# Modelos de Difusão

---

## Introdução

Os modelos de Difusão são uma classe de modelos generativos que funcionam bem na remoção de ruído e na geração de geração de dados. Eles são projetados para aprender a transformar dados ruidosos em dados limpos através de um processo iterativo de denoising.

---

## Conceitos Basicos

- Difusão: Refere-se ao processo gradual de adição de ruido a imagens ou um dado, em seguida reverter este processo para geração de dados limpos a partir do ruido.
- Denoising: Processo de remoção de ruído. Em modelos de difusão, o denoising é feito iterativamente para melhorar a qualidade dos dados gerados. Geralmente utilizando calculos probabilisticos.

![image.png](/assets/image.png)

---

## Tipos de Modelos de Difusão

### Modelos de Difusão Baseados em Cadeias de Markov

- **Modelo de Difusão Simples**: Adiciona ruído gaussiano aos dados em uma sequência de passos e aprende a reverter esse processo.
  - Este é não é tão recomendado para geração multiclasse, pelo fato de como não utiliza uma condicional para guiar o modelo, ele pode gerar estruturas aleatorias.
- **Modelo de Difusão de Langevin**: Utiliza um processo estocástico contínuo para modelar a difusão e a reversão do ruído.

### Modelos de Difusão Latente

- **Difusão Latente**: Trabalha em um espaço latente em vez do espaço original dos dados, o que pode ser mais eficiente em termos de computação.
- **VQ-VAE (Vector Quantized Variational Autoencoder)**: Um tipo de autoencoder que usa quantização vetorial para representar dados latentes.

---

## Modelos de Difusão Condicional

Os Modelos de Difusão Condicional são uma variante avançada dos modelos de difusão que permitem um controle mais preciso sobre o processo de geração. Eles incorporam informações adicionais (condições) para guiar o processo de geração, resultando em saídas mais específicas e controladas.

### Características principais:

- **Controle sobre a geração:** Permite especificar certas características ou atributos desejados no resultado final.
- **Versatilidade:** Pode ser aplicado em diversos domínios, como geração de imagens, texto e áudio.
- **Flexibilidade:** As condições podem ser incorporadas de várias maneiras, como através de embeddings ou características específicas do domínio.

### Aplicações comuns:

- **Geração de imagens condicionadas por texto:** Como no caso do DALL-E e Stable Diffusion, onde o modelo gera imagens baseadas em descrições textuais.
- **Edição de imagens:** Permite modificar aspectos específicos de uma imagem existente.
- **Síntese de voz condicionada:** Geração de fala com características específicas, como tom, emoção ou sotaque.

Os Modelos de Difusão Condicional representam um avanço significativo na área de geração de conteúdo, oferecendo um nível de controle e especificidade que não era possível com os modelos de difusão tradicionais.

---

## Modelos de Difusão Famosos

Um dos modelos famosos na aplicação e para criação de difusão é a u-net (https://arxiv.org/abs/1505.04597), por preservar caracteristicas da imagem em durante todo o treinamento.

![image.png](/assets/image%201.png)

---

## [\*\*Denoising Diffusion Probabilistic Models](https://huggingface.co/papers/2006.11239) (DDPM)\*\*

https://huggingface.co/papers/2006.11239

https://arxiv.org/abs/2006.11239

O processo descrito no paper está relacionado a um processo de geração e denoising de imagens com base em um processo de difusão.

### Processo de Difusão Direta (Forward Process)

Nesta fase, o ruído é gradualmente adicionado à imagem original em T passos de tempo.

- Para cada passo t, temos:
- x*t = √(1 - β_t) \*x*(t-1) + √(β_t)\* ε
- Onde:
  - x_t é a imagem no passo t
  - β_t é a variância do ruído no passo t
  - ε é o ruído gaussiano

### Processo de Difusão Reversa (Backward Process)

Esta fase visa reverter o processo de difusão, removendo gradualmente o ruído para gerar a imagem final.

- Para cada passo t, temos:
- x*(t-1) = 1/√(1 - β_t) *(x_t - β_t/√(1 - α_t)* ε*θ(x_t, t))
- Onde:
  - ε_θ é a rede neural que estima o ruído
  - α_t = 1 - β_t

O modelo é treinado para estimar o ruído em cada passo, permitindo assim a geração de novas imagens a partir do ruído puro.

### Cálculos Importantes

1. α_t = 1 - β_t
2. ᾱ_t = Π(1 - β_i) de i=1 até t
3. Variância posterior q(x\_(t-1)|x_t, x_0) = σ̃_t^2 \* I
4. σ̃*t^2 = β_t \* (1 - ᾱ*(t-1)) / (1 - ᾱ_t)

Estes cálculos são fundamentais para o funcionamento do DDPM, permitindo uma transição suave entre os estados de ruído e a imagem final gerada.
