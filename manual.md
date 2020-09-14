![Image](http://www.mesg.com.br/wp-content/uploads/2020/05/mesglogo.png)
[www.mesg.com.br](http://www.mesg.com.br)

# Envio de SMS via Web

A interação com a ferramenta se dá por requisições HTTP ao endereço

`http://api.mesg.com.br/<função>`

Estão disponíveis as seguintes funções:

- envio de mensagens simples para um celular apenas, função **simplesSMS**
- confere o status de envio de uma mensagem, função **statusSMS**
- envio de mensagens simples para múltiplos celulares, função **multiSMS**
- envio de diferentes mensagens para múltiplos celulares, função **multiMsgMultiSMS**
- consultar crédito, função **saldoSMS**

# Envio de Mensagem simples

Uso mais básico, esta chamada envia uma mensagem SMS de até 160 caracteres para um único celular

Função  **simplesSMS**

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número a ser enviadono no formato ddd+número
- `msg` texto a ser enviado, máximo de 160 caracteres, para quebra de linhas usar `<br>`

### Exemplo de URL
```
http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Titulo<br>Nova linha
```

### Resposta de Retorno
O envio de uma mensagem retorna uma string composta de dois campos separados por ";" (ponto e vírgula).  Exemplo
```
6;291294218
```
O primeiro campo faz referência à operação de envio, no exemplo acima **6** indica que a mensagem foi enviada. A lista completa de retorno está é descrita na tabela a seguir.

Código | Descrição
------ | -------------
1      | Usuário ou senha inválido, ou usuário cancelado
2      | Usuário sem créditos na plataforma
3      | Celular Inválido
4      | Mensagem em branco ou com caracteres inválidos
5      | reservado
6      | Mensagem enviada

O segundo campo retorna um número de identificação da mensagem ( número **291294218** do exemplo). Este código é útil na verificação do status de envio de uma mensagem sendo usada na função **statusSMS**. Veja Próxima seção.

# Status de Mensagem

Função  **statusSMS**

Uma chamada a esta função retorna o status de envio de uma mensagem. Requer  o número de identificação da mensagem gerado na operação de envio.

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `id` código de retorno da mensagem


### Exemplo de URL
```
http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&msg=número-identificação-mensagem
```

### Resposta de Retorno

Código | Descrição
------ | -------------
0      | Login inválido
1      | reservado
2      | reservado
3      | reservado
4      | Mensagem Enviada
5      | Erro no envio
6      | reservado
7      | Sem créditos na Plataforma
8      | reservado
9      | Recebimento confirmado


# Envio de Mensagem simples Para Múltiplos Celulares

Usado para enviar a mesma mensagem para múltiplos celulares

Função  **multiSMS**

### Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` números a serem enviados, separados por `;` (ponto e vírcula) no formato ddd+número
- `msg`máximo de 160 caracteres

### Exemplo de URL
```
http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888;48977777777&msg=Mensagem
```

### Resposta de Retorno

Código | Descrição
------ | -------------
1      | Usuário ou senha invalido, ou usuário cancelado
2      | Usuário sem créditos na plataforma
3      | Celular Inválido
4      | Mensagem em branco ou com caracteres inválidos
5      | reservado
6      | Mensagem enviada

# Envio de Múltiplas mensagens para Múltiplos Celulares

Use este opção para enviar mensagens diferentes para números diferentes

Função  **multiMsgMultiSMS**

### Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` números a serem enviados, separados por `/n` (barra n) no formato ddd+número
- `msg` mensagens a serem enviadas, separadas por `/n` (barra n), máximo de 160 caracteres por mensagem

### Exemplo de URL
```
http://api.mesg.com.br/multiMsgMultiSMS?user=usuario&password=senha&destinatario=48988888888/n48977777777&msg=Mensagem1/nMensagem2
```
### Resposta de Retorno
Código | Descrição
------ | -------------
1      | Usuário ou senha inválido, ou usuário cancelado
2      | Usuário sem créditos na plataforma
3      | Quantidade de mensagens e celulares diferentes, por exemplo 3 mensagens para 4 celulares
4      | Mensagem em branco ou com caracteres inválidos
5      | reservado
6      | Mensagem enviada


# Saldo de Mensagens na plataforma

Consultar o saldo de mensagens disponíveis na plataforma

Função  **saldoSMS**

### Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário

### Exemplo de URL
```
http://api.mesg.com.br/saldoSMS?user=usuario&password=senha

```

### Resposta de Retorno
Código | Descrição
------ | -------------
1      | Usuário ou senha inválido, ou usuário cancelado
2      | Número 2 seguido da quantidade de créditos

## Exemplo em Python
necessário uso da biblioteca [requests](
https://requests.readthedocs.io/pt_BR/latest/user/quickstart.html)
```python
import requests
# simula a requisicao GET de um browser como um URL
requisicao = requests.get("http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Mensagem simples SMS")

# imprime o status de retorno
print(requisicao.text)
```

### Links
[Editor Online Marckdown](https://jbt.github.io/markdown-editor)

[Editor Online Marckdown Stackedit](https://stackedit.io)

[Sintaxe Markdown Github](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf)

[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
