### ⚡ Simulação de Adoção de Energia Solar  
### 🌱 Global Solution – Soluções em Energias Renováveis e Sustentáveis  
### 📚 Ciência da Computação – 2º Semestre / 2025

---

## 👨‍💻 Integrantes

| Nome Completo          | RM       |
|------------------------|----------|
| Patrick Mansour        | RM562970 |
| Pietro Mauer           | RM563676 |
| Samir Assad            | RM561562 |

---

## 📌 Descrição do Projeto

Este projeto implementa uma **simulação do impacto da adoção de energia solar fotovoltaica** em um ambiente de trabalho, estimando:

- Redução do consumo da rede elétrica  
- Economia financeira mensal  
- Redução de emissões de CO₂  
- Autonomia energética  
- Comparação entre cenário atual e cenário com energia renovável  

A solução atende à **Opção C - Simulação de Uso de Energias Renováveis** da Global Solution.

---

## 🎯 Objetivo

Demonstrar como a implementação de energia solar pode reduzir custos, tornar ambientes corporativos mais eficientes e contribuir para um futuro do trabalho mais sustentável e inteligente.

---

## 🧪 Metodologia

### **1. Simulação de Consumo**
Foi gerado um conjunto de dados fictício representando 30 dias de consumo horário de energia em um escritório, com padrões realistas de pico e baixa demanda.

### **2. Simulação de Geração Solar**
Foi criada uma curva de geração solar baseada em um painel de 3 kW, com pico ao meio-dia e eficiência de 85%.

### **3. Cálculo dos Benefícios do Sistema Solar**
A simulação estima:

- Quantidade de energia suprida pela geração solar  
- Redução da dependência da rede elétrica  
- Economia em reais  
- Emissão evitada de CO₂  

---

## 🔧 Tecnologias Utilizadas

- Python 3  
- Pandas  
- NumPy  
- Matplotlib  

---

## 🧠 Código Principal da Simulação

```python
# BLOCO 1 — IMPORTAÇÃO DAS BIBLIOTECAS

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# BLOCO 2 — CRIAÇÃO DAS DATAS DA SIMULAÇÃO
# 30 dias com intervalo de 1 hora (720 pontos) 

horas = pd.date_range("2025-01-01", periods=24*30, freq="h")


# BLOCO 3 — SIMULAÇÃO DO CONSUMO ENERGÉTICO
# Consumo típico de escritório: sobe no dia,
# cai de madrugada. Com pequena variação aleatória.


consumo_base = 2.2 + 1.2 * np.sin(2 * np.pi * (horas.hour - 6) / 24)
consumo = consumo_base + np.random.normal(0, 0.15, len(horas))



# BLOCO 4 — SIMULAÇÃO DE GERAÇÃO SOLAR
# Geração só ocorre durante o dia e faz pico ao meio-dia.


geracao_solar_horaria = np.maximum(0, 3.0 * np.sin(np.pi * (horas.hour - 6) / 12))
geracao_solar = geracao_solar_horaria



# BLOCO 5 — CRIAÇÃO DO DATAFRAME
# Agrupamos todas as séries simuladas em uma tabela.


df = pd.DataFrame({
    "data": horas,
    "consumo": consumo,
    "solar": geracao_solar
})


# BLOCO 6 — CÁLCULOS DE ENERGIA SOLAR
# solar_utilizada = min(consumo, solar)
# rede = consumo - solar_utilizada


df["solar_utilizada"] = df[["consumo", "solar"]].min(axis=1)
df["rede"] = df["consumo"] - df["solar_utilizada"]



# BLOCO 7 — MÉTRICAS GERAIS
# Calcula total consumido, autonomia, economia,


consumo_total = df["consumo"].sum()
solar_utilizada = df["solar_utilizada"].sum()
consumo_rede = df["rede"].sum()

autonomia = (solar_utilizada / consumo_total) * 100

# Economia estimada (tarifa de R$ 0,80 por kWh)
economia = solar_utilizada * 0.80

# Redução de CO₂ (0.085 kg por kWh)
reducao_co2 = solar_utilizada * 0.085

print("\n==== RESULTADOS DA SIMULAÇÃO DE ENERGIA SOLAR ====")
print(f"Consumo total...................: {consumo_total:.2f} kWh")
print(f"Geração solar utilizada.........: {solar_utilizada:.2f} kWh")
print(f"Consumo da rede após solar.....: {consumo_rede:.2f} kWh")
print(f"Autonomia energética............: {autonomia:.1f}%")
print(f"Economia estimada...............: R$ {economia:.2f}")
print(f"Redução de CO₂..................: {reducao_co2:.2f} kg")
print("====================================================\n")




# BLOCO 8 — GRÁFICO (APENAS 1 DIA)


df_1dia = df.iloc[:24]  # somente o primeiro dia

plt.figure(figsize=(12,5))
plt.plot(df_1dia["data"], df_1dia["consumo"], label="Consumo", linewidth=2)
plt.plot(df_1dia["data"], df_1dia["solar"], label="Geração Solar", linewidth=2)

plt.xlabel("Hora do Dia")
plt.ylabel("kWh")
plt.title("Simulação: Consumo vs Geração Solar (1 Dia)")
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

