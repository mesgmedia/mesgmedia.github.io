![Image](http://www.mesg.com.br/wp-content/uploads/2020/05/mesglogo.png)
[www.mesg.com.br](http://www.mesg.com.br)

# Geração de código OTP

A interação com a ferramenta se dá por requisições HTTP ao endereço

`http://api.mesg.com.br/v2/<função>`

Estão disponíveis as seguintes funções:

- gera um código OTP e envia a um celular específico, função **sendOTP**
- checa a validade de um código OTP, função **checkOTP**

# Gerar código OTP

Esta chamada envia uma mensagem SMS de até 160 caracteres para um único celular

Função  **simplesSMS**

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número a ser enviado no no formato ddd+número
- `msg` texto a ser enviado, máximo de 160 caracteres, para quebra de linhas usar `<br>`

### Exemplo de URL
```
http://api.mesg.com.br/v2/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Titulo<br>Nova linha
```

### Resposta de Retorno
O envio de uma mensagem retorna uma string composta de três campos separados por "|" (barra vertical).  Exemplo
```
1701|5548988887777|ba6cce34-8806-4f74-a2da-6533cbe15cec
```
O primeiro campo faz referência à operação de envio, no exemplo acima **1701** indica que a mensagem foi enviada. A lista completa de retorno está é descrita na tabela a seguir.

Código | Descrição
------ | -------------
1701   | Mensagem enviada com sucesso
1702   | URL inválida, um dos parâmetros não foram fornecidos ou estão em branco
1703   | Usuário ou senha inválido, ou usuário cancelado
1705   | Mensagem inválida
1706   | Destinatário inválido
1710   | Erro interno
1025   | Usuário sem créditos na plataforma

O segundo campo **5548988887777** no exemplo acima indica o número que foi enviado a mensagem.
O terceiro campo **ba6cce34-8806-4f74-a2da-6533cbe15cec**  é um número de identificação da mensagem.

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
