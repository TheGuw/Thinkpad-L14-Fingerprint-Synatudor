# ThinkPad L14 Fingerprint Fix (Synaptics 06cb:00be)

Este documento contém o passo a passo completo para habilitar o leitor biométrico Synaptics no Lenovo ThinkPad L14 utilizando o Ubuntu 24.04 LTS ou 26.04 LTS.

---

1. IDENTIFICAÇÃO DO DISPOSITIVO - Certifique-se de que o seu hardware é compatível. Execute:

```bash
lsusb
```

O dispositivo deve aparecer como: "ID 06cb:00be Synaptics, Inc."

---

2. INSTALAÇÃO DO SERVIÇO DE BIOMETRIA - Atualize o sistema e instale os pacotes necessários para o suporte ao fingerprint:

```bash
sudo apt update
```
```bash
sudo apt install -y fprintd libpam-fprintd
```

---

3. HABILITAR AUTENTICAÇÃO NO SISTEMA - Ative o suporte no "PAM" (Pluggable Authentication Modules):

```bash
sudo pam-auth-update --enable fprintd
```
---

4. DOWNLOAD DO DRIVER (synaTudor) - Clone o repositório do driver customizado:

```bash
git clone https://github.com/Popax21/synaTudor.git
```
```bash
cd synaTudor
```

---

5. INSTALAÇÃO DE DEPENDÊNCIAS DE COMPILAÇÃO - Instale todas as ferramentas necessárias para construir o driver a partir do código-fonte:

```bash
sudo apt install -y meson cmake pkg-config libcrypto++-dev libusb-1.0-0-dev libcap-dev libseccomp-dev libglib2.0-dev libdbus-1-dev libfprint-2-dev libfprint-2-tod-dev libjson-glib-dev innoextract libssl-dev libudev-dev systemd-dev
```

---

6. COMPILAR E INSTALAR O DRIVER - Execute o processo de "build" utilizando "meson" e "ninja":

```bash
meson build
```
```bash
cd build
```
```bash
ninja
```
```bash
sudo ninja install
```

---

7. REGISTRAR DIGITAL - Agora você já pode cadastrar sua impressão digital:

```bash
fprintd-enroll
```

DICA: Você também pode gerenciar as digitais pela interface gráfica em "Configurações" > "Usuários" > "Fingerprint Login".

---

8. SOLUÇÃO DE PROBLEMAS (TROUBLESHOOTING)
- Se o sensor não for detectado após a instalação, tente reiniciar o serviço:
```bash
  sudo systemctl restart fprintd
```
- Caso o sistema pare de solicitar a senha, execute:
```bash
sudo pam-auth-update
```
e verifique se as configurações estão corretas.

---
CRÉDITOS:
Método baseado no driver "synaTudor" desenvolvido por Popax21.
