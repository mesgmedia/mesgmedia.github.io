# Envio de SMS via Web

A interação com a ferramenta se dá por requisições HTTP ao endereço

`http://api.mesg.com.br/<função>`

Estão disponíveis as seguintes funções:

envio de mensagens
- **simplesSMS** - simples para um celular apenas, função
- **multiSMS** - simples para múltiplos celulares
- **multiMsgMultiSMS** - diferentes mensagens para múltiplos celulares
- **saldoSMS** - consultar crédito

# Envio de Mensagem simples 

Uso mais básico, esta chamada envia uma mensagem SMS de até 160 caracteres para um único celular

Função  **simplesSMS**

Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número a ser enviadono no formato ddd+número
- `msg`texto a ser enviado, máximo de 160 caracteres, para quebra de linhas usar <br>

Exemplo de URL
``` 
http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Titulo<br>Nova linha
```

## Código de Retorno

First Header | Second Header
------------ | -------------
Content cell 1 | Content cell 2
Content column 1 | Content column 2

# Envio de Mensagem simples Para Múltiplos Celulares

Função  **multiSMS**

Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` números a serem enviados, separados por `;` (ponto e vírcula) no formato ddd+número
- `msg`máximo de 160 caracteres

Exemplo de URL
``` 
http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888;48977777777&msg=Mensagem
```

## Código de Retorno

First Header | Second Header
------------ | -------------
Content cell 1 | Content cell 2
Content column 1 | Content column 2


## Exemplo em Python
necessário uso da biblioteca [requests](
https://requests.readthedocs.io/pt_BR/latest/user/quickstart.html)
```python
import requests
# simula a requisicao GET de um browser como um URL
requisicao = requests.get("http://api.mesg.com.br/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Mensagem simples SMS")
print('get=',requisicao.text)
```

### Links
[Editor Online Marckdown](https://jbt.github.io/markdown-editor)

[Sintaxe Markdown Github](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf)

[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
