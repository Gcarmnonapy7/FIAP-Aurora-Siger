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

| Sensor | μ | σ | Unidade | Referência | Comentário |
|--------|---|---|---------|------------|------------|
| Temperatura interna | 22 | 1.5 | °C | ISS: 18–26°C, média 21–23°C | Ambiente pressurizado com controle térmico ativo |
| Temperatura externa | 10 | 50 | °C | LEO: -65°C a +125°C | Variação extrema entre face solar e sombra da nave |
| Integridade estrutural | — | — | 0/1 | Binário: 1 = íntegro, 0 = comprometido | Sensores binários de ruptura/deformação no casco |
| Energia | 95 | 3 | % | ISS: 84–120 kW, baterias Li-ion de 4 kWh | Percentual de carga das baterias Li-ion |
| Vibração | 0.3 | 0.1 | g | Pré-decolagem: ~0.1–0.5g | Vibrações residuais dos motores em pré-ignição |
| Pressão dos tanques | 4.5 | 0.5 | atm | LOX/LH2 pump-fed: ~3.7–5.5 atm | Pressurização dos tanques criogênicos de LOX/LH2 |

### Fontes

- [ESA — Temperatures on the Space Station](https://www.esa.int/ESA_Multimedia/Images/2021/08/Temperatures_on_the_Space_Station)
- [Sciencing — Temperature of Outer Space Close to Earth](https://www.sciencing.com/1921895/temperature-outer-space-close-earth/)
- [NASA — LH2 Propellant Loading Model](https://c3.ndc.nasa.gov/dashlink/static/media/publication/LH2-AIAA.pdf)
- [ScienceDirect — LOX Tank Pressurisation System](https://www.sciencedirect.com/science/article/abs/pii/S1359431119381049)
- [NASA — Spacecraft Level Vibration Testing](https://ntrs.nasa.gov/api/citations/20150020490/downloads/20150020490.pdf)
- [Wikipedia — ISS Electrical System](https://en.wikipedia.org/wiki/Electrical_system_of_the_International_Space_Station)

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
