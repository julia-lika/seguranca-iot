# Ponderada de Programação (Semana 9): Visão Geral sobre Segurança em IoT

## Grupo 4 Apontados 


## 👨‍🎓 Integrantes: 
<div align="center">
  <table>
    <tr>
      <td align="center"><a href="https://www.linkedin.com/in/fernando-soares-oliveira/"><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Fernando.png" alt="" /><br><sub><b>Fernando Oliveira</b></sub></a></td>
      <td align="center"><a href="https://www.linkedin.com/in/bernardofmeirelles/"><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Bernardo.png" alt="" /><br><sub><b>Bernardo Meirelles</b></sub></a></td>
      <td align="center"><a href="https://www.linkedin.com/in/larissa-temoteo/"><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Larissa.png" alt="" /><br><sub><b>Larissa Temoteo</b></sub></a></td>
      <td align="center"><a href="https://www.linkedin.com/in/j%C3%BAlia-alvesdejesus/"><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Julia.png" alt="" /><br><sub><b>Júlia Alves<//b></sub></a></td>
      <td align="center"><a href="https://www.linkedin.com/in/tainacortez/"><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Taina.png" alt="" /><br><sub><b>Tainá Cortez</b></sub></a></td>
      <td align="center"><a href="https://www.linkedin.com/in/julia-lika-ishikawa/" ><img style="border-radius: 10%; width: 100px; height: 100px;" src="assets/fotos_integrantes/Lika.png" alt="" /><br><sub><b>Julia Lika</b></sub></a></td>
    </tr>
  </table>
</div>


### **Vulnerabilidades Identificadas**
1. **Falta de Criptografia no MQTT**:
   - As mensagens trafegam em texto puro.
   - **Impacto**: Exposição a ataques Man-in-the-Middle.

2. **Configuração Wi-Fi com Credenciais Fracas**:
   - Uso de redes não protegidas ou senhas simples.
   - **Impacto**: Risco de invasão.

3. **Ausência de Validação de Mensagens MQTT**:
   - Mensagens maliciosas podem ser aceitas sem autenticação.
   - **Impacto**: Controle não autorizado do sistema.

4. **Armazenamento Frágil de Digitais**:
   - Modelos biométricos são manipulados sem criptografia.
   - **Impacto**: Violação de dados sensíveis.

---

## **Ataques Simulados**

### **Análise de Tráfego de Rede: Captura de Mensagens MQTT**

#### **Objetivo**
Realizar a interceptação e análise de mensagens publicadas pelo ESP32 no broker MQTT, utilizando o Wireshark, para identificar possíveis vulnerabilidades na comunicação.

---

## **Descrição da Atividade**

### **1. Processo Realizado**
- A análise de tráfego foi conduzida utilizando a ferramenta **Wireshark** para capturar pacotes enviados pelo ESP32.
- O objetivo foi inspecionar as mensagens publicadas no broker MQTT e avaliar se essas mensagens eram trafegadas de forma segura.

---

### **2. Ferramentas Utilizadas**
- **Hardware**:
  - ESP32 conectado a uma rede Wi-Fi.
- **Software**:
  - Wireshark: Ferramenta para captura e análise de tráfego de rede.
- **Protocolo Avaliado**:
  - MQTT na porta padrão `1883` (sem criptografia).

---

## **Procedimentos**

### **Passo-a-Passo da Captura**
1. **Configuração do Wireshark**:
   - Filtro aplicado:
     ```plaintext
     tcp.port == 1883
     ```
   - Este filtro capturou apenas pacotes MQTT trafegando na rede.
   - O ESP32 e o meu computador estiveram logados o tempo todo na mesma rede.

2. **Execução do ESP32**:
   - O ESP32 foi programado para publicar mensagens no tópico `instituto/biometria/cadastro`.

3. **Captura de Pacotes**:
   - Pacotes MQTT foram identificados no Wireshark, com os seguintes detalhes:
     - **Tipo de Mensagem**: `PUBLISH`.
     - **Tópico**: `instituto/biometria/cadastro`.
     - **Payload**:
       ```json
       {"449.408.468-90"}
       ```

4. **Análise**:
   - O payload da mensagem estava trafegando em texto puro, sem criptografia.

---

## **Resultados Obtidos**

### **1. Exposição de Dados**
- As mensagens MQTT capturadas continham informações críticas, como IDs de usuários, em formato texto puro.
- Qualquer dispositivo na mesma rede Wi-Fi pode interceptar e visualizar essas mensagens.

### **2. Vulnerabilidade Identificada**
- **Falta de Criptografia**:
  - As mensagens não estavam protegidas por SSL/TLS, expondo o sistema a ataques de interceptação (Man-in-the-Middle).
---

## **Impacto**

### **1. Confidencialidade**
- Informações sensíveis, como IDs de acesso ou CPF, como no exemplo, podem ser interceptadas por um atacante.

### **2. Possibilidade de Ataques**
- Um atacante pode reutilizar mensagens capturadas (ataque de replay) para simular acessos autorizados.
- Mensagens podem ser manipuladas para alterar o comportamento do sistema.

---
### Imagens

<div align="center">
  <sub>Figura 1 - Imagem do MqttExplorer - Publicando mensagem</sub><br>
  <img src="./assets/mqtt.jpg" width="600px" height="auto"><br>
  <sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>

<div align="center">
  <sub>Figura 2 - Imagem do WireShark</sub><br>
  <img src="./assets/cpfin.jpg" width="600px" height="auto"><br>
  <sup>Fonte: Material produzido pelos autores (2024)</sup>
</div>


---
