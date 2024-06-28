#dev #react #dev/front

É um hook do React 18 para gerenciar transições e animações de forma eficaz, evitando gargalos de desempenho e melhorando a experiência do usuário. Em outras palavras, permite você atualizar o estado sem bloquear a UI.

O seu uso é bem simples:

```javascript
export function Actions({ isFollowing, userId }: ActionsProps) {

  const [isPending, startTransition] = useTransition(); //1. criar o hook
  const handleFollow = () => {

    startTransition(() => { //2. chamar a função
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
   //3. usar o estado
    <Button disabled={isPending} onClick={onClick} variant="primary"> 
      {isFollowing ? "Unfollow" : "Follow"}
    </Button>
  );
}
```

Ele é uma forma bacana de substituir os `setLoading(true)` e `setLoading(false)` para bloquear um botão enquanto uma atividade assíncrona está rolando por exemplo, ou *InfiniteScrolling*.

Nesse exemplo, a função `startTransition` executa uma [[Action]] chamada `onFollow()`.