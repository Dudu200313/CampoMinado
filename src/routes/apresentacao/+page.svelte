<body>
    <div class="container">
        <h1>Apresentação do Jogo</h1>
        <p>
            Bem-vindo ao emocionante mundo do nosso jogo! Aqui, você assumirá o
            controle de um carrinho que deve navegar por um mapa tridimensional
            repleto de desafios. O objetivo é conduzir o carrinho do ponto de
            partida ao ponto de chegada, mas tenha cuidado com os obstáculos ao
            longo do caminho.
        </p>
        <p>Recursos do Jogo:</p>
        <ul>
            <li>Mapas personalizados e desafiantes</li>
            <li>Tijolos especiais com surpresas</li>
            <li>Bombas perigosas e campos de força protetores</li>
            <li>Tome decisões estratégicas para evitar danos</li>
            <li>Exibição de informações em tempo real</li>
        </ul>
        <p>
            Está pronto para encarar o desafio? Clique no botão abaixo para
            começar!
        </p>
        <a href="/" class="start-button">Voltar a tela inicial</a>
    </div>
</body>

<style>
    body {
        font-family: "Arial", sans-serif;
        background-color: #f0f0f0;
        margin: 0;
        padding: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }

    .container {
        width: 100%;
        max-width: 800px;
        padding: 20px;
        background-color: #fff;
        border: 2px solid #007bff;
        border-radius: 10px;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
        text-align: center;
    }

    h1 {
        font-size: 36px;
        color: #007bff;
        margin-bottom: 20px;
    }

    p {
        font-size: 18px;
        color: #333;
        line-height: 1.6;
    }

    /* Estilos para o botão fictício */
    .start-button {
        display: inline-block;
        padding: 12px 24px;
        background-color: #007bff;
        color: #fff;
        font-size: 18px;
        text-decoration: none;
        border-radius: 5px;
        transition: background-color 0.3s;
        cursor: pointer;
    }

    .start-button:hover {
        background-color: #0056b3;
    }

    /* Estilos para a lista */
    ul {
        text-align: left;
        margin-top: 20px;
        padding-left: 20px;
    }

    li {
        font-size: 16px;
        color: #555;
        line-height: 1.4;
        list-style-type: disc; /* Adiciona um ponto na frente dos itens */
        margin-bottom: 10px; /* Espaçamento entre os itens */
    }
</style>
