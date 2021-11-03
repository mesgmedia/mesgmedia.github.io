![Image](http://www.mesg.com.br/wp-content/uploads/2020/05/mesglogo.png)

# API de SMS One Time Password (OTP)

A interação com a ferramenta se dá por requisições HTTP ao endereço

`http://api.mesg.com.br/v2/<função>`

Estão disponíveis as seguintes funções:

- **sendOTP** gera um número OTP e envia a um celular específico
- **checkOTP** checa a validade de um número OTP

# Gerar Número OTP

Esta chamada envia uma mensagem SMS a um celular específico contendo um número OTP.

Função  **SendOTP**

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número celular o qual será enviado o número OTP,  formato ddd+número
- `msg` texto a ser enviado, máximo de 160 caracteres. A  mensagem de template deverá ter o caracter `"%m"`. o número OTP gerado toma esta posição na mensagem final. A mensagem dever ser URL encoded.
- `tamanho` tamanho do número OTP (até 9 caracteres)
- `tempo` tempo de validade do número OTP em segundos
### Exemplo de URL
```
http://api.mesg.com.br/v2/sendOTP?user=xxxx&password=yyyy&destinatario=48988887777&msg=&Seu número de acesso a Mesg: %m&tamanho=6&tempo=480
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

Uma chamada a esta função retorna o status de uma mensagem  OTP enviado. Requer  o número OTP recebido pelo usuário na operação de envio.

### Parâmetros
- `user` login do usuário na plataforma Mesg
- `password` senha deste usuário
- `detinatario` número celular que foi enviado o número OTP
- `otp` número OTP recebido a ser validado.



### Exemplo de URL
```
http://api.mesg.com.br/v2/checkOTP?user=xxxx&password=yyyy&destinatario=48988887777&otp=435687
```
Se o número OTP for validado um código **101** é retornado indicando sucesso da operação.  A lista completa dos códigos de retorno estão descritas na tabela a seguir.

### Resposta de Retorno

Código | Descrição
------ | -------------
101      | número OTP validado com sucesso
102      | OTP expirado
103      | reservado
104      | número celular não encontrado
1702      | um dos parâmetros faltando ou OTP não é numérico
1703      | falha na autentificação
[comment]: <> (1706 - xxx? ver diferença com 104 )
