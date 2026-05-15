```mermaid
---
config:
  theme: mc
  layout: fixed
---
flowchart LR
%%  subgraph FB1["Fallback"]
%%         F1{"Primary #2<br>available?"}
%%         F2["Select Primary #2<br>Next non-Manual held"]
%%         F3[/"Release all held orders<br>trigger_scraping each"/]
%%   end
%%  subgraph B["Phase B — AIaaS Processing"]
%%         B1["AIaaS call\nPrimary #1"]
%%   end
%%  subgraph C["Phase C — AIaaS Processing"]
%%         C1["AIaaS call<br>Primary #2"]
%%   end
%%  subgraph D["Phase D — Result Sharing"]
%%         D1["Survey match held orders<br>PB: strip --- · HR: direct · RJ: sub-survey"]
%%         D2["Inherit results<br>Copy scraper + AIaaS output"]
%%         D3["Unmatched orders<br>Release → individual scraping"]
%%   end
    START(["All orders created successfully"]) --> ELIG{"State in [PB, HR, RJ]<br>& count(non-manual orders) > 1"}
    ELIG --NO--> BYP[/"Process individually (all/remaining)"/]
    ELIG --YES--> A["Trigger scraping for primary #1, keep rest on HOLD"]
    A --> SC1{"Scraper result<br>primary #1?"}
    SC1 -- FAIL --> F1{"Primary #2<br>available?"}
    F1 -- YES --> F2["Select Primary #2<br>Next non-Manual held"]
    F1 -- NO --> BYP
    SC1 -- OK --> B["AIaaS call<br>Primary #1"]
    F2 --> SC2{"Scraper result<br>Primary #2?"}
    SC2 -- FAIL --> BYP
    SC2 -- OK --> C1["AIaaS call<br>Primary #2"]
    B --> AI1{"AIaaS result <br> primary #1?"}
    C1 --> AI2{"AIaaS result<br>Primary #2?"}
    AI1 -- FAIL --> F1{"Primary #2<br>available?"}
    AI2 -- FAIL --> BYP
    AI1 -- OK --> D1["Survey match for held orders<br>PB, HR, RJ"]
    AI2 -- OK --> D1
    D1 --> D2{"Unmatched orders left?"}
    D2 -- NO -->D3["Inherit results<br>Copy scraper + AIaaS output"]
    D3 --> DONE(["COMPLETED<br>All orders processed"])
    D2 -- YES-->F1
    BYP --> DONE


    classDef Sky stroke-width:1px, stroke-dasharray:none, stroke:#374D7C, fill:#E2EBFF, color:#374D7C
    style A font-size:13px
    style F1 font-size:13px
    style F2 font-size:13px
%%     style F3    fill:#FEE2E2,stroke:#B91C1C,color:#B91C1C,font-size:13px
    style B font-size:13px
    style C1 font-size:13px
    style D1 font-size:13px
    style D2 font-size:13px
    style D3 font-size:13px
    style START font-size:13px
    style ELIG font-size:13px
    style BYP   fill:#FEE2E2,stroke:#B91C1C,color:#B91C1C,font-size:13px
    style SC1 font-size:13px
    style SC2 font-size:13px
    style AI1 font-size:13px
    style DONE  fill:#0F5132,stroke:#0F5132,color:#fff,font-size:13px
    linkStyle 5 stroke:#000000,fill:none
  ```
