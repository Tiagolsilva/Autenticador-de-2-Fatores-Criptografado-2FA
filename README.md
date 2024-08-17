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
pip install pyotp qrcode[pil] cryptography
```

## Uso

1. **Gerar a chave HOTP e QR Code:**

   O código gera uma chave HOTP e cria um QR Code associado a ela. O QR Code é salvo como "QRCODE.png".

2. **Criptografia e Descriptografia:**

   O código gera um código HOTP, criptografa-o com 4 camadas de criptografia, e depois descriptografa o código para verificar a integridade.

3. **Execução em Loop:**

   O código roda em um loop infinito, gerando e criptografando códigos HOTP a cada 30 segundos, e verifica se a descriptografia está funcionando corretamente.

## Código

```python
import pyotp
import qrcode
from cryptography.fernet import Fernet
import base64
import time

# Gerar chave HOTP
secret = pyotp.random_base32()
hotp = pyotp.HOTP(secret)

# Gerar o QRCODE
otp_uri = hotp.provisioning_uri(name="User@example.com", initial_count=0, issuer_name="NameExample")
qr = qrcode.make(otp_uri)
qr.save("QRCODE.png")
print("QRCODE gerado")

# Criando 4 camadas de criptografia diferentes com Fernet
keys = [Fernet.generate_key() for _ in range(4)]
cipher_layers = [Fernet(key) for key in keys]

def generate_and_encrypt_code(counter):
    otp_code = hotp.at(counter)
    print(f"OTP original gerado (contador {counter}): {otp_code}")

    encrypted_code = otp_code.encode()  # Inicialmente, converta o código HOTP para bytes

    # Criptografia do código com múltiplas camadas
    for cipher in cipher_layers:
        encrypted_code = cipher.encrypt(encrypted_code)

    encrypted_b64 = base64.b64encode(encrypted_code).decode('utf-8')
    print(f"Código HOTP criptografado com 4 camadas: {encrypted_b64}")
    
    return encrypted_b64

def decrypt_code(encrypted_code):
    # Decodificando de base64
    encrypted_code = base64.b64decode(encrypted_code)
    
    # Descriptografia do código para revelar o HOTP original
    decrypted_code = encrypted_code
    for cipher in reversed(cipher_layers):
        decrypted_code = cipher.decrypt(decrypted_code)
    
    # Converta o código descriptografado de volta para string
    final_code = decrypted_code.decode()
    print(f"Código HOTP após descriptografia: {final_code}")
    
    return final_code

# Loop para gerar e criptografar o código a cada 30 segundos
counter = 0
while True:
    encrypted_code = generate_and_encrypt_code(counter)
    decrypted_code = decrypt_code(encrypted_code)
    
    # Verificação de integridade
    if decrypted_code == hotp.at(counter):
        print("Descriptografia bem-sucedida, código original restaurado.")
    else:
        print("Erro na descriptografia, código não corresponde.")
    
    counter += 1
    time.sleep(30)  # Espera 30 segundos para gerar o próximo código
```

## Contribuindo

Se você quiser contribuir para este projeto, sinta-se à vontade para fazer um fork e enviar pull requests. Sugestões e melhorias são sempre bem-vindas!
 Se precisar de mais informações ou ajustes, é só me avisar!
