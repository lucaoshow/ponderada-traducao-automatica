
# Neural Machine Translation with RNNs (Seq2Seq)

Este projeto implementa um pipeline simples de tradução automática (inglês→francês) inspirado em D2L Seção 9.5.

## Conteúdo
- `NMT_Seq2Seq_Colab.ipynb`: Notebook Colab com:
  - Download e pré-processamento (ManyThings/Tatoeba, fra-eng.zip)
  - Tokenização e vocabulários com tokens especiais (<pad>, <bos>, <eos>, <unk>)
  - Truncamento/preenchimento e minibatches
  - Modelo Seq2Seq (Encoder/Decoder GRU) com treinamento rápido
  - Decodificação gulosa para avaliação qualitativa
  - Experimento de tamanho de vocabulário variando `num_examples`

## Como executar (Colab)
1. Abra o notebook no Google Colab e selecione GPU (opcional).
2. Execute todas as células em ordem. O último bloco gera este README automaticamente.

## Resultados (resumo)
- Perplexidade por época exibida no treinamento (ver notebook).
- Traduções de exemplo impressas (qualitativas).

## Exercícios (D2L 9.5.7)

### 1) Variação de `num_examples` e tamanhos de vocabulário

Pergunta: Tente valores diferentes do argumento num_examples na função load_data_nmt.
Como isso afeta os tamanhos do vocabulário do idioma de origem e do idioma de destino?

Resposta (baseada nos resultados deste notebook):

- num_examples=300: src_vocab=102, tgt_vocab=107
- num_examples=600: src_vocab=184, tgt_vocab=201
- num_examples=1000: src_vocab=266, tgt_vocab=321
- num_examples=5000: src_vocab=875, tgt_vocab=1231
- num_examples=10000: src_vocab=1505, tgt_vocab=2252
- num_examples=20000: src_vocab=2459, tgt_vocab=3828

Conclusão: conforme aumentamos num_examples, ambos os vocabulários crescem monotonicamente (ou quase),
pois mais sentenças expõem mais tipos de palavras. As taxas de crescimento diferem por idioma
devido a diferenças morfológicas e distribuição de tokens no corpus.

### 2) Tokenização em idiomas sem separadores de palavras (chinês/japonês)

Pergunta: O texto em alguns idiomas, como chinês e japonês, não tem indicadores de limite de palavras.
A tokenização em nível de palavra ainda é uma boa ideia para esses casos? Por que ou por que não?

Resposta:
Não. Em línguas sem separadores explícitos de palavras (p.ex., chinês, japonês), a tokenização em nível de palavra
exige uma etapa externa de segmentação que é ruidosa e dependente de dicionário. Isso pode introduzir erros sistêmicos
no treinamento. Em vez disso, tokenização em nível de subpalavra (BPE/WordPiece/Unigram, como em Sennrich et al., 2016; Kudo, 2018)
ou mesmo nível de caractere pode ser preferível, pois lida melhor com morfologia rica e OOV.

Referências:
- D2L 9.5 destaca que o vocabulário em nível de palavra cresce muito, e sugere técnicas de tokenização mais avançadas.
- Sennrich, Haddow, Birch (2016): Neural Machine Translation of Rare Words with Subword Units.
- Kudo (2018): Subword Regularization: Improving Neural Network Translation Models with Multiple Subword Candidates.
- Devlin et al. (2019): BERT uses WordPiece (subword) tokenization, amplamente adotado em PLN.

