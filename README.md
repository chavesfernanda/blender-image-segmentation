# Geração de Máscaras de Segmentação de Instância com Blender 2.83

Este repositório contém a implementação prática do processo de geração automática de **máscaras de segmentação de instância** (*Instance Segmentation Masks*) utilizando o Blender 2.83. A técnica é utilizada para a criação de datasets sintéticos voltados ao treinamento de modelos de Visão Computacional e Inteligência Artificial.

---

## Estrutura do Repositório

```text
.
├── assets/                           # Imagens de documentação e capturas de tela
│   ├── My_test1/                     # Evidências do projeto/cena própria
│   └── video_aula/                   # Evidências do acompanhamento do tutorial
├── files/                            # Arquivos nativos do Blender (.blend)
│   ├── geo_ambiente.blend            # Arquivo com a cena customizada e composição de nós
│   └── my_test.blend                 # Arquivo de testes e experimentos
├── my_test/                          # Datasets sintéticos gerados (Cena Personalizada)
│   ├── Image0001.png                 # Renderização RGB principal
│   ├── Segmentation0001.png          # Máscara do Objeto Index 1 (Cubo)
│   ├── Segmentation20001.png         # Máscara do Objeto Index 10000 (Cilindro)
│   └── Segmentation30001.png         # Máscara do Objeto Index 20000 (Cone)
├── tutorial/                         # Datasets sintéticos gerados (Cena do Vídeo)
│   ├── Image0001.png                 # Renderização RGB principal
│   ├── Segmentation0001.png          # Máscara do Objeto Index 1
│   ├── Segmentation20001.png         # Máscara do Objeto Index 27
│   └── Segmentation30001.png         # Máscara do Objeto Index 200
└── README.md                         # Documentação principal
```
---
## Metodologia e Fluxo de Trabalho

Todo o pipeline foi desenvolvido utilizando exclusivamente as ferramentas nativas e o Compositor de Nós do *Blender 2.83* com o motor de renderização Cycles.

**A. Configuração da Cena e Atribuição de Índices**

1. O motor de renderização foi alterado para Cycles (GPU/CPU Compute) para habilitar o suporte aos passes avançados de renderização.

2. Cada objeto da cena recebeu um identificador exclusivo através do parâmetro `Pass Index` em `Object Properties -> Relations`:

- Cubo (Base): `Pass Index = 1`

- Cilindro (Intermediário): `Pass Index = 10000`

- Cone (Topo): `Pass Index = 20000`

3. Na aba `View Layer Properties -> Passes`, ativou-se a opção Object Index (`IndexOB`), tornando o canal de índices disponível no Compositor.

2. Árvore de Nós no Compositor

A composição dos nós foi estruturada para permitir a exportação simultânea da imagem colorida e das máscaras binárias isoladas:

- *Nó Render Layers:* Fornece as saídas de cor (`Image`) e do canal de índices (`IndexOB`).

- *Nó Math (Divide):* Divide a saída `IndexOB` por `65535` para normalização em escala de cinza de 8/16 bits (permitindo representar até 65.536 objetos únicos).

- *Nós ID Mask:* Filtram a imagem e extraem máscaras binárias limpas (sem anti-aliasing) para cada objeto com base nos índices `1`, `10000` e `20000`.

- *Nó File Output:* Grava automaticamente a imagem RGB e as máscaras geradas nas pastas de destino (`my_test/` e `tutorial/`) no momento da renderização (F12).

---
## Conclusões

- A extração de máscaras via canal IndexOB garante limites de pixel exatos sem contaminação por anti-aliasing, essencial para o treinamento de modelos de IA.

- O uso de formatos de 8/16 bits permite escalar a criação de datasets sintéticos para simulações complexas com milhares de instâncias.

- A exportação via nó File Output automatiza 100% da criação do dataset no próprio processo de renderização do Blender.
---
**Autor(a):**
FERNANDA DE SOUSA CHAVES
