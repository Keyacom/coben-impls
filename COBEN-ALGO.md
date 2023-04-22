# The COBEN algorithm

**COBEN** stands for ***C**hance **O**f **B**eing **E**liminated **N**ext*.
It determines the relative chance of being eliminated next based on the competitors'
current scores and what scores they can gain after the next event.

## Description

1. Getting input
    1. Let $S$ be the list of current scores, or a table, with the keys being
    the contestants' names, and the values being the scores.
    2. Let $N$ be the list of scores for the next event.
        - If the scores are not static, let $N$ be $N_i = m$, where $m$ is the
        maximum score that can be received in this event.
    3. Assert that $S$ and $N$ have equal length. If the assertion fails, exit
    early.
2. Calculating the COBEN
    1. Setup
        1. Let $r$ be $\bar{S} + \bar{N}$, with the top bar denoting arithmetic
        mean of the list's (or table's) values.
        2. Let $D$ be the list (or table) of inverse differences, defined as
        $D_i = \max\left(0, \dfrac{1}{S_i} - \dfrac{1}{r}\right)$.
        3. Let $C$ be an empty list (or table).
    2. For each $s$ of $S$:
        1. If $s \ge r$, append immunity for
        $\left\lfloor\dfrac{s}{r}\right\rfloor$ events to $C$.
        2. Otherwise, append COBEN equal to
        $\dfrac{100\\\%}{s \times \sum D_i}$ to $C$. ($\sum D_i$ is the sum of
        $D$'s values.)
        <!--
        Double backslash is required due to the fact the GFM parser interprets
        escapes in math expressions.
        -->
3. Output $C$.

## Representation

### Flow chart

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    START([Start])
    subgraph SG_INPUT [Get input]
        START-->G_SC[/"scores = (Get current scores as list of integers)"/]-->G_SCE
        G_SCE[/"nextscores = (Get scores for next event as list of integers)"/]-->ASSERT_EQ_LEN
        ASSERT_EQ_LEN{{"(length of current scores) == (length of scores for next event)"}}
    end
    ASSERT_EQ_LEN-->|Yes|SG_CALC
    ASSERT_EQ_LEN-->|No|END
    subgraph SG_CALC [Calculate]
        subgraph SG_IMMREQ [Immunity requirement]
            OUT_IR[/"(Mean of scores) + (Mean of nextscores) as float"/]
        end
        subgraph SG_COBENS [Chances of being eliminated next]
            OUT_IR-->G_IREQ[/ireq = Get immunity requirement/]
            G_IREQ-->INIT_CHANCES[chances = empty array]
            INIT_CHANCES-->INIT_INVDIFS[inverted_differences = empty array]-->POPULATE_INVDIFS
            POPULATE_INVDIFS-->COND_NEXT_SCORE{{"score = (Get next item of scores)"}}
            COND_NEXT_SCORE-->|Obtained|COND_SCORE_GE_IREQ{{score >= ireq}}
            COND_SCORE_GE_IREQ-->|Yes|APPEND_IMM_TO_CHANCES["Append immunity for (score / ireq as integer) events to chances"]
            COND_SCORE_GE_IREQ-->|No|SG_SCORE_LT_IREQ-->COND_NEXT_SCORE
            APPEND_IMM_TO_CHANCES-->COND_NEXT_SCORE
        end
        subgraph SG_SCORE_LT_IREQ [If the score is less than the immunity requirement]
            APPEND_COBEN_TO_CHANCES["Append COBEN of (score's corresponding inverted_differences item /\n/ sum of inverted_differences * 100%) to chances"]
        end
        subgraph POPULATE_INVDIFS [Populate inverted differences]
            APPEND_INVDIF_TO_CHANCES["Append (1 / score) - (1 / ireq) to inverted_differences"]-->BIND_INVDIF_SCORE[Bind each inverted_differences item to each score]
        end
    end
    subgraph SG_OUTPUT [Yield output]
        COND_NEXT_SCORE-->|Not obtained|DISP
    end
    DISP[/Output chances/]-->END
    END([End])
```
