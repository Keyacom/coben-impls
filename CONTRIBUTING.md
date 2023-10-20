# Contribution guidelines

1. Your project must implement the COBEN algorithm.
2. Due to this project's lax licensing, **do not include any implementation
directly within this repository!** Instead, link your implementation's repository
in /IMPLEMENTATIONS.md, and link this repository in yours. The preferred way is the badge: [![COBEN implementation](https://img.shields.io/static/v1?message=implementation&style=for-the-badge&label=COBEN&color=%23E83828)](https://github.com/Keyacom/coben-impls)
3. Your implementation may or may not include support for contestants' names.
4. Requirements for testing the implementation:
    - Input (current scores): 292, 123, 73, 51, 34
    - Input (scores for next event): 200, 150, 100, 50, 0
    - Output (does not have to follow the below text format):

      ```txt
      292   Immune (1)
      123    6.65% COBEN
      73    17.31% COBEN
      51    28.63% COBEN
      34    47.41% COBEN
      ```

      It's not imperative for the COBEN percentages to follow
      the above sample output exactly due to the nature of binary floating-point
      numbers, but the difference from the expected and actual result should
      be preferably less than 0.01.

## Badge source in Markdown

```md
[![COBEN implementation](https://img.shields.io/static/v1?message=implementation&style=for-the-badge&label=COBEN&color=%23E83828)](https://github.com/Keyacom/coben-impls)
```
