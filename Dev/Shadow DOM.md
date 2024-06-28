#dev #dev/front #web 


O Shadow DOM permite anexar uma nova árvore [[DOM]] ("árvore fantasma"), na DOM principal. Isolando o CSS e a estrutura do componente do resto do documento, evitando conflitos de estilo e comportamento. Ou seja, ele encapsula componentes que podem ser reutilizados.

![[Pasted image 20240628175204.png]]

### Modo aberto/fechado

Um Shadow DOM pode ter dois modos: aberto e fechado.
	**Aberto:** é possível acessar a *shadow root* do exterior usando JS.
	**Fechado:** o *shadow root* não é acessível
## Exemplo

```html
<!DOCTYPE html> 
<html lang="en"> 
	<head> 
		<meta charset="UTF-8"> 
		<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
		<title>Exemplo de Web Component</title> 
	</head> 
	<body>
		<meu-componente></meu-componente>
	</body> 
</html>
```

```javascript
class MeuComponente extends HTMLElement {
      constructor() {
        super();
        // Criação do shadow root
        const shadow = this.attachShadow({ mode: 'open' });

        // Criação do conteúdo do shadow DOM
        const container = document.createElement('div');
        container.innerHTML = `
          <style>
            p {
              color: blue;
            }
          </style>
          <p>Olá, eu sou um Web Component!</p>
        `;

        // Anexação do conteúdo ao shadow root
        shadow.appendChild(container);
      }
    }
```