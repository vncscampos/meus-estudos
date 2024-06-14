#dev #rect

É o processo de comparação entre o HTML estático gerado no servidor e o HTML gerado pelo React no cliente em aplicações SSR. Se os dois HTMLs forem correspondente beleza, mas se não, pode dar **hydration error** em dev e até bugs em prod, pois o React vai fazer mínimas alterações para sincronizar os dois documentos e que pode dar um BO danado.

## Exemplo

Em um código de clone do Twitch, ao adicionar um o componente do React *Suspense* acontecia um *hydration error* (dessa parte do suspense eu não entendi porque :s).

```javascript
export default function BrowseLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <>
      <Navbar />
      <div className="flex h-full pt-20">
        <Suspense fallback={<SidebarSkeleton />}>
          <Sidebar />
        </Suspense>
        <Container>{children}</Container>
      </div>
    </>
  );
}
```

O *Suspense* serve para suspender a renderização de um componente até que certas condições sejam atendidas. Nesse caso, como *Sidebar* faz chamada de dados assíncronos, até que esses dados seja carregados, seria exibido o *SidebarSkeleton*.

Como mostra o código abaixo, *collapsed* recebe *false* o que significa que por padrão a *Sidebar* não é fechada (ou colapsada).

```javascript
import { create } from "zustand";

interface SidebarStore {
  collapsed: boolean;
  onExpand: () => void;
  onCollapse: () => void;
}

export const useSidebar = create<SidebarStore>((set) => ({
  collapsed: false,
  onExpand: () => set(() => ({ collapsed: false })),
  onCollapse: () => set(() => ({ collapsed: true })),
}));
```

No componente da *Sidebar*, temos o *Wrapper*

```javascript
export async function Sidebar() {
  const recommended = await getRecommended();

  return (
    <Wrapper>
      <Toggle />
      <div className="space-y-4 pt-4 lg:pt-0">
        <Recommended data={recommended} />
      </div>
    </Wrapper>
  );
}

export function SidebarSkeleton() {
  return (
    <aside className="fixed left-0 flex flex-col w-[70px] lg:w-60 h-full bg-background border-r border-[#2D2E35] z-50">
      <ToggleSkeleton />
      <RecommendedSkeleton />
    </aside>
  );
}
```

```javascript
"use client";

interface WrapperProps {
  children: React.ReactNode;
}

export function Wrapper({ children }: WrapperProps) {
  const { collapsed } = useSidebar((state) => state);
  return (
    <aside className={cn("fixed left-0 flex flex-col w-60 h-full bg-background border-r border-[#2d2e35] z-50", collapsed && "w-[70px]")}>
      {children}
    </aside>
  );
```

O *Wrapper* é um componente do cliente que tem o `width` ajustado dinamicamente para `70px` quando o `collapsed=true`. O problema é que o servidor não tem informação do `collapsed` retornando um estado com *Sidebar* aberta, enquanto o cliente sabe qual valor dessa variável e sabe que para mobile esse valor deve ser `true`, criando um outro estado de *Sidebar* fechada p/ mobile.

Resumindo o problema, enquanto o servidor gera um HTML com sidebar aberta, o cliente gera um HTML com sidebar fechada para resoluções mobile, resultando no *hydration error*.

## Solução

A solução para esse problema é fazer o servidor e o cliente retornarem a mesma coisa, no nosso caso dois skeletons e um `width` estático. E só depois de montado, o componente irá poder realizar as alterações do `useEffect()` no cliente e fazer valer as alterações dinâmicas do `width`.

```javascript
"use client";
export function Wrapper({ children }: WrapperProps) {
  const [hasMounted, setHasMounted] = useState(false);
  const { collapsed } = useSidebar((state) => state);

  useEffect(() => {
	  setHasMounted(true);
  },[]);

  if (!hasMounted)
    return (
      <aside className="fixed left-0 flex flex-col w-[70px] lg:w-60 h-full bg-background border-r border-[#2D2E35] z-50">
        <ToggleSkeleton />
        <RecommendedSkeleton />
      </aside>
    );

  return (
    <aside
      className={cn(
        "fixed left-0 flex flex-col w-60 h-full bg-background border-r border-[#2D2E35] z-50",
        collapsed && "w-[70px]"
      )}
    >
      {children}
    </aside>
  );
}
```

Em NextJS, já existe um *hook* para isso e não precisa do `useEffect()`. É o `useIsClient` que vai funcionar da mesma forma.

```javascript
"use client";

import { useIsClient } from "usehooks-ts";

interface WrapperProps {
  children: React.ReactNode;
}

export function Wrapper({ children }: WrapperProps) {
  const isClient = useIsClient();
  const { collapsed } = useSidebar((state) => state);
  
  if (!isClient)
    return (
      <aside className="fixed left-0 flex flex-col w-[70px] lg:w-60 h-full bg-background border-r border-[#2D2E35] z-50">
        <ToggleSkeleton />
        <RecommendedSkeleton />
      </aside>
    );

	//resto do código
```

#### Info bônus:

Os *hooks* só funcionam no lado do cliente e em **NextJS** para eles funcionarem precisa dizer em cima `"use client"` que aquele é um componente do cliente.