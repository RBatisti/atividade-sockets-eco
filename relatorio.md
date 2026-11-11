# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Sistemas Distribuídos
**Participante:** Rogerio Batisti Junior
**Data:** 11/11/2026

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

Descreva com suas palavras o que cada chamada abaixo faz e por que ela
é necessária:

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | cria o socket e passa como parâmetros o protocolo desejado (por exemplo, IPV4 ou IPV6) e o tipo de conexão, se é TCP ou UDP. |
| `servidor_socket.bind((HOST, PORTA))` | associa o socket criado a um endereço (IP) e a uma porta, que são onde o servidor vai receber as conexões. |
| `servidor_socket.listen(5)` | deixa o socket escutando e define até 5 como o tamanho da fila de conexões. |
| `servidor_socket.accept()` | bloqueia a execução até um cliente se conectar e então aceita a conexão, devolvendo um novo socket (`conn`) para conversar com esse cliente e o endereço dele. |
| `conn.recv(TAMANHO_BUFFER)` | recebe dados mandados pelo cliente com um tamanho máximo definido em `TAMANHO_BUFFER`. |
| `conn.sendall(resposta.encode("utf-8"))` | envia todos os dados da resposta codificada em utf8. |
| `conn.close()` | encerra toda a conexão com esse cliente. |

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | cria o socket do cliente, e, como no servidor, passa como parâmetros o protocolo desejado (por exemplo, IPV4 ou IPV6) e o tipo de conexão, se é TCP ou UDP. |
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | tenta se conectar com o socket criado ao endereço e porta passada. |
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | envia todos os dados da mensagem codificada em utf8. |
| `cliente_socket.recv(TAMANHO_BUFFER)` | bloqueia a execução até receber dados do servidor, lendo no máximo `TAMANHO_BUFFER` bytes. |

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_ porque o `docker-compose.yml` cria o hostname para o servidor como `servidor` e quando é mandado alguma requisição para ele, é convertido para o ip interno definido pelo docker automaticamente. Caso tentássemos acessar com outro nome, não seria possível estabelecer conexão, pois não saberia associar ao IP correto. Caso tentasse acessar o servidor com o nome `localhost` daria o erro `ConnectionRefusedError: [Errno 111] Connection refused`, porque dentro do contêiner do cliente, `localhost` é o próprio cliente e não há nada escutando na porta 5000 ali.

---

## Nível 2 — Modificar o servidor

Descreva as mudanças que você fez no `servidor.py` para:

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_ foi adicionado o `.upper()` na variável `resposta`. Isso é necessário para converter a mensagem recebida para maiúsculas.

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_ a variável `total_mensagens` é incrementada em 1 a cada mensagem recebida e um novo log exibe o valor acumulado. Assim é possível ver quantas mensagens o servidor já recebeu desde que foi iniciado.

Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_ o código é copiado para dentro da imagem durante o `docker build`. Como foram realizadas modificações no `servidor.py`, o `--build` é necessário para gerar uma nova imagem com o código atualizado. Sem ele, o contêiner subiria com a imagem antiga e continuaria executando o código antigo.

---

## Observações livres

Anote aqui qualquer comportamento inesperado que observaram, erros que
apareceram e como resolveram, ou dúvidas que ainda têm:

>

---

## Dúvida para a próxima aula

Escreva **uma pergunta** que surgiu durante a atividade e que vocês
gostariam de ver respondida na aula seguinte:

>
