# Detecção e classificação de crimes cibernéticos em redes sociais utilizando algoritmos de aprendizado de máquina

Repositório para o projeto ***"Detecção e classificação de crimes cibernéticos em redes sociais utilizando algoritmos de aprendizado de máquina"***. 
Este projeto está inserido na disciplina [ACH0042 - Resolução de Problemas II](https://uspdigital.usp.br/jupiterweb/obterDisciplina?sgldis=ACH0042).

A detecção e classificação de discurso de ódio em textos ocorre através de uma **cascata heterogênea em duas fases**: uma triagem rápida (TF-IDF + SVM linear) seguida de um classificador especialista (BERTimbau).

## Contexto

A crescente utilização das redes sociais transformou esses ambientes em espaços
férteis para a disseminação de crimes cibernéticos, incluindo crimes de ódio como
racismo, homofobia, misoginia e intolerância religiosa. A identificação automatizada
desse conteúdo é dificultada pela ambiguidade da linguagem natural, pelo uso de
gírias, ironia e variações ortográficas empregadas para burlar filtros automáticos.
Este trabalho propõe a detecção e classificação de crimes de ódio em textos de redes
sociais, combinando processamento de linguagem natural e aprendizado de máquina em
duas etapas: detecção de tendências maliciosas e classificação em categorias de
crimes de ódio, com avaliação comparativa de diferentes algoritmos. A literatura
revisada mostra predominância de abordagens binárias e uma lacuna entre taxonomias
jurídicas de crimes cibernéticos e métodos computacionais em português, que este
trabalho busca preencher.

## Conjunto de Dados

Para realização desse projeto, o conjunto de dados originalmente utilizado é o
[[Portuguese Hate Speech Expanded Dataset (TuPyE)]](https://github.com/Silly-Machine/TuPyE-Expanded-Brazilian-Hate-Speech-Dataset).
O conjunto foi modificado para a aplicação de algoritmos de aprendizado de máquina.

## Estrutura
 
| Arquivo | Conteúdo |
|---|---|
| `1_Dois_Rotulos.ipynb` | Análise exploratória do dataset **Binary** (`aggressive`, `hate`), cascata SVM + BERTimbau com validação e tabelas de resultados |
| `2_Treze_Rotulos.ipynb` | Análise exploratória do dataset **Multilabel** (13 rótulos), cascata de 13 rótulos, ROC-AUC, matrizes de confusão, *self-attention* e ablação da perda ponderada |
| `3_Simulacao_Oliveira.ipynb` | Baselines de estágio único (BERTimbau sem SVM) para reproduzir os resultados de Oliveira et al. (2024) e compará-los com a cascata, com teste de McNemar |

## Ordem de execução e dependências entre notebooks
 
Rode na ordem **1 → 2 → 3**. Os notebooks 1 e 2 são independentes entre si; o 3 consome os resultados dos dois por arquivos na pasta `resultados/`.
 
```
1_Dois_Rotulos  ──► resultados/cascata_2rotulos.npz  ──┐
                                                        ├──► 3_Simulacao_Oliveira
2_Treze_Rotulos ──► resultados/cascata_13rotulos.npz ──┘
```

| Arquivo | Gravado por | Lido por | Conteúdo |
|---|---|---|---|
| `resultados/cascata_2rotulos.npz` | Notebook 1 (última célula) | Notebook 3, Parte A | `y_true_total`, `pred_bert_total`, `probabilidade_crime_svm`, `limiar_svm_calibrado` |
| `resultados/cascata_13rotulos.npz` | Notebook 2 (célula "Exportar resultados") | Notebook 3, Parte C e McNemar | `Y_total`, `y_pred_casc`, `idx_val`, `limiar_svm_calibrado` |


### Persistência no Google Colab

A pasta `resultados/` fica no disco temporário da sessão e **é perdida se o ambiente reiniciar**. Para persistir, monte o Google Drive e altere `PASTA_RESULTADOS` para uma pasta do Drive. O valor deve ser o **mesmo nos três notebooks** (célula "Exportar resultados" nos notebooks 1 e 2, e célula "Resultados da cascata" no notebook 3).

## Tempo estimado (GPU T4)
 
Valores aproximados, calculados a partir das execuções feitas e de estimativas (o SVM não teve o tempo medido):
 
| Notebook | Estimativa |
|---|---|
| 1 | cerca de 20 a 30 min (a Fase 2 binária treina até 5 épocas) |
| 2, sem a ablação | cerca de 30 a 40 min (a Fase 2 de 3 épocas levou cerca de 15 min) |
| 2, ablação | **mais de 2 h** (10 treinos); rode por último |
| 3 | cerca de 1 h (três baselines) |
 


## Contribuidores

- [Caio Bernardo](https://github.com/caio-bernardo)
- [Lucas Neves](https://github.com/fatorarpolinomio)
- [Willian Farias](https://github.com/willianjsf)

## Licença

[CC0 1.0 Universal License](LICENSE) &copy;
