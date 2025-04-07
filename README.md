# 📷 Reconhecimento de Dígitos com Raspberry Pi Pico W (MNIST + OLED + LEDs + Bluetooth)

Este projeto implementa um sistema embarcado completo de reconhecimento de dígitos usando uma imagem em formato MNIST. Ele roda localmente no **Raspberry Pi Pico W** com firmware MicroPython, utilizando uma interface interativa com:

- Display OLED
- Matriz de LEDs RGB (5x5)
- Joystick e botões físicos
- Comunicação Bluetooth BLE com celular

---

## 🚀 Visão Geral

O sistema permite ao usuário percorrer imagens de dígitos usando o joystick, visualizar a imagem no OLED e, ao pressionar o botão A, realizar a inferência local do dígito. O resultado é mostrado na matriz de LEDs. Em seguida, o usuário pode enviar o resultado via BLE para um dispositivo conectado pressionando o botão B.

---

## 🔧 Especificações do Sistema

### 🔌 Firmware e Ambiente
- **Firmware MicroPython**: v1.24.1
- **Pico SDK**: v2.0.0
- **Toolchain**: ARM GCC 10.x

### 📚 Bibliotecas utilizadas
- `aioble`: para comunicação Bluetooth BLE
- `bluetooth`: nativo do MicroPython
- `uasyncio`: para multitarefa assíncrona
- `ssd1306.py`: biblioteca para controle do display OLED via I2C

### 🧩 Periféricos conectados
| Periférico         | Função                       | GPIO(s)       |
|---------------------|-------------------------------|---------------|
| Display OLED (SSD1306) | Exibição da imagem MNIST       | GPIO14 (SDA), GPIO15 (SCL) |
| Matriz de LEDs RGB (NeoPixel 5x5) | Exibição do resultado ou animações | GPIO7 |
| Botão A             | Confirmar a escolha (iniciar inferência) | GPIO5         |
| Botão B             | Enviar resultado via BLE       | GPIO6         |
| Joystick horizontal | Navegar entre imagens          | GPIO26 (VRx)  |
| LED RGB discreto    | Indicar status de conexão BLE  | GPIO11, 12, 13|

---

## 🧠 Como funciona o sistema

1. **Inicialização**:
   - Display OLED e matriz de LED são apagados.
   - O LED RGB inicia em **vermelho**, indicando que não há conexão BLE ativa.

2. **Conectar ao BLE**:
   - A placa entra em modo de anúncio BLE com nome `PicoMNIST`.
   - Ao conectar via celular (ex: aplicativo BLE Scanner), o LED RGB fica **azul**.

3. **Escolher o número**:
   - Use o **joystick** para esquerda/direita para navegar entre as imagens `imagem_X.txt`.
   - A imagem é exibida no display OLED.

4. **Inferência do dígito (botão A)**:
   - Pressione o **botão A** para iniciar a inferência MNIST.
   - A matriz de LED mostra `!` indicando processamento.
   - A inferência é feita localmente via algoritmo `predict()` com pesos e bias pré-carregados.
   - Após a inferência, o resultado é exibido na matriz de LEDs (0 a 9).

5. **Enviar resultado via BLE (botão B)**:
   - Pressione o **botão B** para mostrar um `X` na matriz LED e enviar o número via BLE.
   - O número é enviado como um `uint8` através de uma característica GATT.

6. **Repetir o processo**:
   - Use o joystick para mudar a imagem e repetir o ciclo.

---

## 📁 Estrutura de arquivos sugerida

```
/
├── main.py                 # Arquivo principal
├── display_lcd.py         # Controle do OLED
├── matriz_led.py          # Funções para LED RGB e matriz
├── mnist_inferencia.py    # Inferência do modelo MNIST
├── ssd1306.py             # Biblioteca OLED
├── weights.bin            # Pesos do modelo (binário)
├── biases.txt             # Biases do modelo (texto)
├── imagem_1.txt           # Imagens MNIST simuladas
├── imagem_2.txt
├── ...
```

---

## 📲 Como testar a conexão BLE
1. Baixe um app como **nRF Connect** ou **BLE Scanner** no celular.
2. Ligue a placa e verifique se o dispositivo `PicoMNIST` aparece.
3. Conecte ao dispositivo. O LED RGB ficará **azul**.
4. Aperte o botão A para inferir, e depois o botão B para enviar.
5. Leia o valor enviado na característica BLE.

---

## 📦 Futuras melhorias
- Exibir o resultado no OLED também.
- Enviar histórico de resultados via BLE.
- Adicionar suporte a entrada de imagem desenhada no joystick.
- Criar interface visual no celular (app).
