# 🌌 Arte Generativa: Rede Neural Interativa

![Licença: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Status: Concluído](https://img.shields.io/badge/Status-Concluído-success.svg)
![Tecnologia: HTML5/JS](https://img.shields.io/badge/Tecnologia-HTML5%20%2F%20JS-F7DF1E.svg)

Uma visualização interativa e relaxante que simula a conexão de neurônios ou constelações em tempo real. Desenvolvido puramente com **HTML5 Canvas**, **CSS3** e **JavaScript vanilla** (sem frameworks), focado em manipulação gráfica e lógica de animação matemática.

## ✨ Demonstração

[-> Clique aqui para ver a arte ao vivo! <-](https://brendowjanuzzi-ui.github.io/arte-generativa-neural/)

![Animação da Rede Neural](demo-neural.gif)

## 🧠 Como Funciona?

O projeto utiliza a **Canvas API** do HTML5 para desenhar e animar centenas de "partículas" (pontos) que se movem de forma aleatória em um espaço 2D. A "mágica" acontece na lógica matemática que conecta esses pontos:

1.  **Geração:** Partículas são criadas com posições (`x`, `y`) e velocidades (`vx`, `vy`) aleatórias.
2.  **Animação:** A cada *frame* (usando `requestAnimationFrame`), as posições são atualizadas. Se uma partícula toca a borda da tela, sua velocidade é invertida (efeito de "rebater").
3.  **Conexões (Matemática):** O código calcula a **distância Euclidiana** entre *todas* as partículas. Se a distância for menor que o limite definido, uma linha é desenhada entre elas. A opacidade da linha depende da proximidade (mais perto = mais opaca).
4.  **Interatividade (Física Simples):** O mouse atua como uma força de atração/repulsão suave. Quando o mouse está próximo, as partículas alteram sua trajetória na direção dele.
5.  **Responsividade:** O sistema detecta mudanças no tamanho da janela (`resize` event) e redistribui as partículas instantaneamente.

## 🛠️ Tecnologias Utilizadas

* **HTML5:** Estrutura e elemento `<canvas>`.
* **CSS3:** Layout do painel de controle e efeitos de *blur* no fundo.
* **JavaScript (ES6):**
    * **Canvas API:** Manipulação direta de pixels, caminhos (`paths`) e preenchimentos.
    * **Lógica Vetorial:** Cálculos de distância e movimento.
    * **Event Listeners:** Interação com o mouse e redimensionamento da janela.

## ⚙️ Funcionalidades do Painel

Você pode interagir com a animação através do painel de controle:

* **Pontos:** Ajusta a quantidade de neurônios na tela (entre 50 e 350).
* **Conexão:** Altera a distância máxima para que dois pontos formem uma conexão.
* **Cores Aleatórias:** Gera uma nova paleta de cores *neon* para as partículas.

## 📄 Licença

Este projeto está sob a licença MIT. Consulte o arquivo [LICENSE](LICENSE) para obter mais detalhes.

---
Desenvolvido com 💜 por Brendow Januzzi
