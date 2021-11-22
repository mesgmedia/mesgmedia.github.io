![Image](http://www.mesg.com.br/wp-content/uploads/2020/05/mesglogo.png)
[www.mesg.com.br](http://www.mesg.com.br)
API v2.

A interação com a ferramenta se dá por requisições HTTPS ao endereço

`https://api.mesg.com.br/v2/<função>`

Estão disponíveis as seguintes funções:

- **simplesSMS** envio de mensagens simples para um celular apenas
- **saldoSMS** consultar crédito
- **sendOTP** gera um número OTP e envia a um celular específico
- **checkOTP** checa a validade de um número OTP

# Envio de Mensagem simples

Esta chamada envia uma mensagem SMS de até 160 caracteres para um único celular

Função  **simplesSMS**

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número a ser enviado no no formato ddd+número
- `msg` texto a ser enviado, máximo de 160 caracteres

### Exemplo de URL
```
https://api.mesg.com.br/v2/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=mensagem
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


# Consulta Saldo de Mensagens

Consulta o saldo de mensagens sms disponíveis na plataforma

Função  **saldoSMS**

### Parâmetros
- `user`  login do usuário na plataforma Mesg
- `password` senha deste usuário

### Exemplo de URL
```
https://api.mesg.com.br/v2/saldoSMS?user=usuario&password=senha
```

### Resposta de Retorno
Informa a quantidade disponível de sms na plataforma.

# Gerar Número OTP

Esta chamada envia uma mensagem SMS a um celular específico contendo um token OTP.

Função  **SendOTP**

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número celular o qual será enviado o número OTP,  formato ddd+número
- `msg` texto a ser enviado, máximo de 160 caracteres. A  mensagem de template deverá ter o caracter `"%m"`. o token OTP gerado toma esta posição na mensagem final. A mensagem dever ser URL encoded.
- `tamanho` tamanho do número OTP (até 9 caracteres)
- `tempo` tempo de validade do número OTP em segundos
### Exemplo de URL
```
https://api.mesg.com.br/v2/sendOTP?user=xxxx&password=yyyy&destinatario=48988887777&msg=&Seu número de acesso a Mesg: %m&tamanho=6&tempo=480
```

### Resposta de Retorno
O envio bem sucedido de uma mensagem retorna uma string composta de três campos separados por "|" (barra vertical).  Exemplo
```
1701|5548988887777|ba6cce34-8806-4f74-a2da-6533cbe15cec
```
O primeiro campo faz referência à operação de envio, no exemplo acima **1701** indica que a mensagem foi enviada com sucesso. A lista completa dos códigos de retorno estão descritas na tabela a seguir.

Código | Descrição
------ | -------------
1701   | Mensagem enviada com sucesso
1702   | URL inválida, um dos parâmetros não foram fornecidos ou estão em branco
1703   | Falha na autenticação do usuário
1705   | Mensagem não contém "%m"
1706   | Destinatário inválido
1710   | Erro interno
1025   | Usuário sem créditos na plataforma

O segundo campo **5548988887777** no exemplo acima indica o número que foi enviado a mensagem.
O terceiro campo **ba6cce34-8806-4f74-a2da-6533cbe15cec**  é um número de identificação da mensagem.

# Validação do Número OTP

Função  **checkOTP**

Uma chamada a esta função retorna o status de uma mensagem OTP enviado. Requer o token OTP recebido pelo usuário na operação de envio.

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número celular que foi enviado o número OTP
- `otp` token OTP recebido a ser validado.



### Exemplo de URL
```
https://api.mesg.com.br/v2/checkOTP?user=xxxx&password=yyyy&destinatario=48988887777&otp=435687
```
Se o token OTP for validado corretamente um código **101** é retornado indicando sucesso da operação.  A lista completa dos códigos de retorno estão descritas na tabela a seguir.

### Resposta de Retorno

Código | Descrição
------ | -------------
101      | token OTP validado com sucesso
102      | OTP expirado
103      | reservado
104      | número celular não encontrado
1702      | um dos parâmetros faltando ou OTP não é numérico
1703      | falha na autentificação
[comment]: <> (1706 - xxx? ver diferença com 104 )

### Exemplo de chamada URL
```
https://api.mesg.com.br/v2/checkOTP?user=xxxx&password=yyy&destinatario=48988887777&otp=435687
```


## Exemplo em Python enviando sms Simples
Este exemplo usa a linguagem Python para enviar uma mensagem por sms. Necessário uso da biblioteca [requests](
https://requests.readthedocs.io/pt_BR/latest/user/quickstart.html)

```python
import requests
# simula a requisicao GET de um browser como um URL
requisicao = requests.get("
https://api.mesg.com.br/v2/simplesSMS?user=usuario&password=senha&destinatario=48988888888&msg=Mensagem simples SMS")
# imprime o status de retorno
print(requisicao.text)
```

## Exemplo de token OTP em PHP
Neste exemplo é gerado um token OTP de 6 dígitos com duração de 7 minutos  que será enviado ao número fictício 48988887777. Neste período de tempo a mensagem precisa ser validada com uma chamada da função **checkOTP**.
```php
<?php
$user="xxxx";
$password="yyyy";
$destinatario="48988887777";
$msg="Seu numero de acesso: %m";
$tamanho=6;  //número gerado será de 6 dígitos
$tempo=420;  //com validade de 7 minutos (420 segundos)

$result = file_get_contents("http://api.mesg.com.br/v2/sendOTP?user=".$user."&password=".$password."&destinatario=".$destinatario."&msg=".urlencode($msg)."&tamanho=".$tamanho."&tempo=".$tempo);

echo $result;

```

### Links
[Editor Online Marckdown](https://jbt.github.io/markdown-editor)

[Editor Online Marckdown Stackedit](https://stackedit.io)

[Sintaxe Markdown Github](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf)

[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
