# Sistema de Autenticação de Dois Fatores com Criptografia Múltipla

Este projeto demonstra um sistema de autenticação de dois fatores (2FA) usando HOTP (HMAC-Based One-Time Password) com criptografia múltipla. Ele utiliza a biblioteca `pyotp` para gerar códigos HOTP e a biblioteca `cryptography` para criptografar e descriptografar esses códigos com múltiplas camadas de criptografia.

## Funcionalidades

- **Geração de Código HOTP:** Gera um código HOTP baseado em um contador.
- **Criação de QR Code:** Cria um QR Code para o código HOTP usando a biblioteca `qrcode`.
- **Criptografia e Descriptografia:** Aplica criptografia múltipla aos códigos HOTP gerados e os descriptografa para verificação.
- **Verificação de Integridade:** Verifica se o código descriptografado corresponde ao código original.

## Dependências

Certifique-se de ter as seguintes bibliotecas instaladas:

```bash
pip install pyotp qrcode cryptography
```

## Uso

1. **Gerar a chave HOTP e QR Code:**

   O código gera uma chave HOTP e cria um QR Code associado a ela. O QR Code é salvo como "QRCODE.png".

2. **Criptografia e Descriptografia:**

   O código gera um código HOTP, criptografa-o com 4 camadas de criptografia, e depois descriptografa o código para verificar a integridade.

3. **Execução em Loop:**

   O código roda em um loop infinito, gerando e criptografando códigos HOTP a cada 30 segundos, e verifica se a descriptografia está funcionando corretamente.

## Contribuindo

Se você quiser contribuir para este projeto, sinta-se à vontade para fazer um fork e enviar pull requests. Sugestões e melhorias são sempre bem-vindas!
 Se precisar de mais informações ou ajustes, é só me avisar!
