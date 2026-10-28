# Classificação de doenças em folhas de tomate por características de cor, textura e forma

Trabalho I da disciplina **Introdução à Visão Computacional** (PUCRS, Escola Politécnica). Pipeline de visão computacional **clássica** (sem deep learning) sobre imagens de folhas de tomate do PlantVillage.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lucasnk1/tomate-classificacao-visao-classica/blob/main/notebook/PlantVillage_VisaoComputacional.ipynb)

## Resumo

Um pipeline em OpenCV segmenta a folha e as lesões e extrai **173 características** (cor, textura e forma). Três classificadores (SVM-RBF, Random Forest e MLP) são avaliados com validação cruzada estratificada de 5 partições em **4.873 imagens** (até 500 por classe), em duas tarefas: saudável ou doente, e 10 classes.

| Tarefa | Melhor modelo | Acurácia | F1 macro |
| --- | --- | --- | --- |
| Saudável ou doente | SVM-RBF | 98,8% | 0,968 |
| 10 classes de tomate | SVM-RBF | 88,9% | 0,891 |

Na tarefa binária, responder sempre "doente" já acerta 89,7%. Os resultados tendem a ser otimistas por causa de quase duplicatas no dataset (ver limitações).

## Pipeline

1. Leitura e redimensionamento (256×256)
2. Pré-processamento: gaussiano 5×5, HSV, Lab e cinza (alimenta cor e textura)
3. Segmentação da folha: distância de cromaticidade ao fundo + Otsu + remoção de sombra + morfologia
4. Segmentação das lesões: dominância do verde g = (G − R) / (R + G + B)
5. Características: histograma HSV, médias e desvios (HSV e Lab), GLCM, LBP, razão de lesão e momentos de Hu
6. StandardScaler → SVM-RBF, Random Forest e MLP
7. Validação cruzada estratificada (5 partições, semente 42)

Ferramentas: OpenCV, NumPy, scikit-image, scikit-learn.

## Como reproduzir

O dataset **não** está neste repositório. Abra o notebook no Colab (botão acima) e execute as células em ordem: a seção 2 baixa o PlantVillage do Kaggle com `kagglehub`. A extração das características leva alguns minutos.

## Estrutura

- `notebook/`: notebook executado, com as saídas
- `paper/`: artigo em formato SBC (PDF e fonte LaTeX)
- `docs/`: apresentação interativa (`index.html`), publicada em https://lucasnk1.github.io/tomate-classificacao-visao-classica/

## Limitações

- O PlantVillage tem imagens quase idênticas da mesma folha; a validação sorteia por imagem, e não por folha.
- Fotos de laboratório com fundo uniforme: a segmentação depende disso e não foi testada em campo.
- Limiares de segmentação definidos empiricamente, sem máscaras de referência.
- Hiperparâmetros fixados sem busca; sem desvio-padrão entre partições.

## Autores

Johann Honscha Cibils e Lucas Leuck de Oliveira.

## Dataset e referência

Hughes, D. P.; Salathé, M. *An open access repository of images on plant health to enable the development of mobile disease diagnostics*. arXiv:1511.08060, 2015. Confira a licença do dataset antes de redistribuir qualquer imagem.

## Licença

Ver o arquivo `LICENSE`.
