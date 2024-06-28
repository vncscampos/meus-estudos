#dev #dev/front #react #nextjs

As **actions** são funções assíncronas que rodam no lado do servidor em resposta a uma interação do usuário no cliente. Essas funções são uma forma rápida e segura de lidar com conexões de banco, trabalhar com tokens, processamento com dados sensíveis etc.

A forma de usar é bem simples, basta adicionar a diretiva `"use server"` na sua **action**.

```javascript
"use server";

import { revalidatePath } from "next/cache";

export async function onFollow(id: string) {
  try {
    const followedUser = await followUser(id);

    revalidatePath("/");

    if (followedUser) {
      revalidatePath(`/${followedUser.following.username}`);
    }

    return followedUser;
  } catch (error) {
    throw new Error("Internal Error");
  }
}
```

Nas **server actions** é comum ao mexer com dados que são refletidos no cliente, atualizar esses dados que estão em cache através da função [[revalidatePath()]].

A chamada dessa função é feito em um componente no cliente com `"use client"`.

```javascript
"use client";
export function Actions({ isFollowing, userId }: ActionsProps) {
  const [isPending, startTransition] = useTransition();
  const handleFollow = () => {

    startTransition(() => {
      onFollow(userId)
        .then((data) =>
          toast.success(`You are now following ${data.following.username}`)
        )
        .catch(() => toast.error("Something went wrong"));
    });
  };

  const onClick = () => {
    if (isFollowing) handleUnfollow();
    else handleFollow();
  };

  return (
    <Button disabled={isPending} onClick={onClick} variant="primary"> 
      {isFollowing ? "Unfollow" : "Follow"}
    </Button>
  );
}
```

Esse é um conceito de chamar uma função em outro endereço é conhecido como [[Remote Procedure Call]].