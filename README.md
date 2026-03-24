# Aurora SIGER

Sistema Inteligente de Gerenciamento de Riscos para telemetria de decolagem espacial.

> Projeto desenvolvido como atividade integradora da **Fase 1** do curso de Ciência da Computação — **FIAP (2026)**.

---

## Sobre o projeto

O Aurora SIGER simula o monitoramento de sensores de uma estação espacial antes da decolagem. A partir de dados de telemetria — temperatura, integridade estrutural, energia, pressão e vibração — o sistema:

1. Organiza e descreve os dados de telemetria
2. Aplica um algoritmo de verificação pré-decolagem (**"PRONTO PARA DECOLAR"** ou **"DECOLAGEM ABORTADA"**)
3. Calcula a autonomia energética da missão
4. Utiliza modelos de Machine Learning (Isolation Forest, One-Class SVM) para detecção de anomalias
5. Apresenta uma reflexão crítica sobre ética, impacto social e sustentabilidade

---

## Estrutura do repositório

```
FIAP-Aurora-Siger/
├── Aurora_siger.ipynb   # Notebook principal com código, análises e resultados
├── README.md            # Este arquivo
├── LICENSE              # Licença do projeto
└── assets/              # Prints de execução (screenshots)
```

---

## Prints da execução

<!-- Adicionar screenshots após execução do notebook -->
<!-- Exemplo: ![Descrição](assets/nome-do-print.png) -->

*Em breve: capturas de tela dos resultados do notebook.*

---

## Como executar

### Opção 1 — Google Colab (recomendado)

1. Clique no badge abaixo para abrir direto no Colab:

   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Gcarmnonapy7/FIAP-Aurora-Siger/blob/main/Aurora_siger.ipynb)

2. Execute todas as células em sequência (`Runtime` → `Run all`)

### Opção 2 — Localmente

```bash
# Clone o repositório
git clone https://github.com/Gcarmnonapy7/FIAP-Aurora-Siger.git
cd FIAP-Aurora-Siger

# Crie um ambiente virtual (opcional, mas recomendado)
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows

# Instale as dependências
pip install numpy pandas seaborn matplotlib scikit-learn jupyter

# Abra o notebook
jupyter notebook Aurora_siger.ipynb
```

---

## Tecnologias utilizadas

| Tecnologia | Uso no projeto |
|------------|----------------|
| Python 3 | Linguagem principal |
| NumPy | Geração de dados e operações numéricas |
| Pandas | Manipulação e análise de dados tabulares |
| Seaborn / Matplotlib | Visualização de dados |
| Scikit-learn | Modelos de detecção de anomalias |
| Jupyter Notebook | Ambiente de desenvolvimento interativo |

---

## Parâmetros de telemetria

Os dados sintéticos foram calibrados com base em referências reais de estações espaciais e veículos de lançamento:

| Coluna | μ | σ | Unidade | Referência | Comentário |
|--------|---|---|---------|------------|------------|
| `internal_temp` | 22 | 1.5 | °C | ISS: 18–26°C, avg 21–23°C | Ambiente pressurizado com controle térmico ativo |
| `external_temp` | 10 | 8 | °C | LEO: -65°C a +125°C | Variação extrema entre face solar e sombra da nave |
| `structural_integrity` | — | — | 0/1 | 1 = íntegro, 0 = falha | Bernoulli(1 - failure_prob): degrada com pressão alta |
| `energy` | 98 | 2 | % | Carga pré-lançamento via GSE, Go/No-Go ≥95% | Normal(98,2) clipped [0,100] — baterias mantidas por GSE |
| `vibration` | 0.3 | 0.1 | g | Pré-decolagem: ~0.1–0.5g | Vibrações residuais dos motores em pré-ignição |
| `tank_pressure` | 305 | 15 | atm | LOX/LH2 pump-fed: 270–340 atm | Pressurização dos tanques criogênicos de LOX/LH2 |
| `critical_modules` | — | — | 0/1 | 1 = ativo, 0 = inativo | Bernoulli(1 - failure_prob): degrada com pressão alta |

### Fontes — Telemetria

- [ESA — Temperatures on the Space Station](https://www.esa.int/ESA_Multimedia/Images/2021/08/Temperatures_on_the_Space_Station)
- [Sciencing — Temperature of Outer Space Close to Earth](https://www.sciencing.com/1921895/temperature-outer-space-close-earth/)
- [NASA — LH2 Propellant Loading Model](https://c3.ndc.nasa.gov/dashlink/static/media/publication/LH2-AIAA.pdf)
- [ScienceDirect — LOX Tank Pressurisation System](https://www.sciencedirect.com/science/article/abs/pii/S1359431119381049)
- [NASA — Spacecraft Level Vibration Testing](https://ntrs.nasa.gov/api/citations/20150020490/downloads/20150020490.pdf)
- [Wikipedia — ISS Electrical System](https://en.wikipedia.org/wiki/Electrical_system_of_the_International_Space_Station)

---

## Parâmetros energéticos

Os parâmetros da análise energética (entregável 1.4) foram calibrados com base em sistemas reais de potência de espaçonaves e veículos de lançamento:

### Capacidade total das baterias (kWh)

| Sistema | Capacidade | Tipo de bateria |
|---------|------------|-----------------|
| Crew Dragon (SpaceX) | ~10–20 kWh | Li-ion (sem painéis solares durante lançamento) |
| Orion (NASA) | ~11 kWh | Li-ion (backup + picos, complementado por painéis solares) |
| Soyuz | ~5–8 kWh | Prata-zinco (módulo de descida) |
| Estágio superior Falcon 9 | ~3–5 kWh | Li-ion |
| ISS (referência) | ~600 kWh | Li-ion (48 unidades, upgrade 2017–2021) |

**Valor na simulação:** 18 kWh (sistema combinado cápsula + estágio superior)

### Carga pré-lançamento e limiar Go/No-Go

- Baterias mantidas a **100%** via Ground Support Equipment (GSE) até desconexão do umbilical (T-2 a T-5 min)
- Carga no momento da desconexão: **~98–100%**
- Limiar mínimo Go/No-Go: **≥95%** (missões tripuladas: ≥97%)

### Consumo elétrico durante a fase de lançamento

| Subsistema | Consumo (W) | Notas |
|------------|-------------|-------|
| Computadores de bordo (redundantes) | 200–600 | Tripla redundância (ex: Falcon 9 usa 3× flight computers) |
| Navegação e controle (GNC) | 100–300 | IMUs, star trackers, receptores GPS |
| Comunicação e telemetria | 100–400 | Downlink S-band/C-band, range safety |
| Atuadores de válvulas e controle de motor | 200–800 | Pico durante eventos de staging |
| Instrumentação e sensores | 50–200 | Pressão, temperatura, vibração |
| Circuitos pirotécnicos | 50–200 (pico) | Separação de estágios, ejeção de carenagem |
| Controle ambiental (tripulado) | 200–500 | Ventiladores, bombas de refrigeração |
| **Total (veículo tripulado)** | **~1.5–3.0 kW** | |

- Duração da fase de lançamento: ~8–10 min até inserção orbital
- Energia elétrica total consumida: **~0.30 kWh**
- Picos de potência (staging): **3–5 kW** por alguns segundos
- Barramento padrão: **28 VDC** (MIL-STD-704)

### Consumo orbital (pós-lançamento)

Em órbita, atuadores de motor e pirotécnicos estão inativos, reduzindo o consumo para **~0.8–1.5 kW** (~50–65% do lançamento):

| Subsistema | Lançamento (W) | Órbita (W) | Notas |
|------------|---------------|------------|-------|
| Computadores de bordo | 200–600 | 200–400 | Mesma redundância, menor carga computacional |
| GNC | 100–300 | 50–150 | Modo cruzeiro, sem correções agressivas |
| Comunicação/telemetria | 100–400 | 100–300 | Downlink contínuo a menor taxa |
| Atuadores/controle de motor | 200–800 | **0** | Motores desligados |
| Pirotécnicos | 50–200 | **0** | Já utilizados no staging |
| Controle ambiental | 200–500 | 200–500 | Mantém-se (suporte de vida) |
| Gerenciamento térmico | — | 100–300 | Aumenta (ciclos sol/sombra ~90 min) |

**Valor na simulação:** 1.2 kW (Crew Dragon free-flight: ~1.0–1.2 kW)

### Cálculo de autonomia

```
energia_útil       = capacidade × carga% × (1 - perdas%)     → 18 × 1.0 × 0.86 = 15.48 kWh
energia_lançamento = consumo_kw × duração_min / 60            → 2.0 × 9/60       = 0.30 kWh
autonomia_orbital  = (energia_útil - energia_lançamento) / consumo_orbital
                   = (15.48 - 0.30) / 1.2 = 12.65 horas
```

### Perdas energéticas

| Tipo de perda | % típica | Descrição |
|---------------|----------|-----------|
| Resistência interna da bateria | 2–5% | Perdas I²R, maiores em altas taxas de descarga |
| Conversão DC-DC | 3–8% | Conversores para barramentos secundários (5V, 12V) com 92–97% de eficiência |
| Resistência de cabeamento | 1–3% | Chicotes elétricos de 100+ metros em alumínio/cobre |
| Gerenciamento térmico | 2–5% | Aquecedores de bateria e sistemas de refrigeração |
| Condicionamento de potência | 1–2% | Reguladores de tensão, filtros, circuitos de proteção |
| **Total combinado** | **~8–18%** | **Valor na simulação: 14%** |

### Fontes — Energia

- Wertz, J. R., Everett, D. F. & Puschell, J. J. — *Space Mission Engineering: The New SMAD* (Cap. 11 — Power Systems)
- Patel, M. R. — *Spacecraft Power Systems* (CRC Press)
- [NASA — ISS Li-ion Battery Upgrade Fact Sheet](https://www.nasa.gov/mission_pages/station/research/benefits/iss-battery-upgrade)
- [SpaceX — Crew Dragon Press Kit (Demo-2)](https://www.spacex.com/launches/)
- [MIL-STD-704F — Aircraft/Spacecraft Electrical Power Characteristics](https://quicksearch.dla.mil/qsDocDetails.aspx?ident_number=36237)
- [ESA — Soyuz Crew Training Manual (Electrical Systems)](https://www.esa.int/Science_Exploration/Human_and_Robotic_Exploration/International_Space_Station)
- [ULA — Atlas V / Centaur User's Guide](https://www.ulalaunch.com/docs/default-source/rockets/atlasv-usersguide2010a.pdf)

---

## Autores

| Nome | RM | Contato |
|------|----|---------|
| Gabriel Carmona Bittencourt | | gabrielcarmonabittencourtpy@gmail.com |
| Samuel Felipe Furtado Freire | | nekolorful@gmail.com |
| Marcio Francisco dos Santos Junior | | marciofsantos65@gmail.com |
| Iúri Leão de Almeida | RM570215 | iurileao@gmail.com |
| Miguel Moreira | | miguelitomoreiraa2008@gmail.com |

---

## Licença

Distribuído sob a licença MIT. Veja [LICENSE](LICENSE) para mais detalhes.
