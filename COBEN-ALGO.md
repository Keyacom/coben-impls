# The COBEN algorithm

**COBEN** stands for ***C**hance **O**f **B**eing **E**liminated **N**ext*.
It determines the relative chance of being eliminated next based on the competitors'
current scores and what scores they can gain after the next event.

<!--
## Description

TODO: Description
-->

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
