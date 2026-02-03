# Raadplegen van VerzoekAanbieder en Verzoek door de Aanbieder (UCLR-0002-ZA)

```mermaid
---
config:
  theme: mc
  look: classic
  layout: elk
---
flowchart LR
 subgraph s1["Leveringsregister"]
          B["Raadplegen VerzoekAanbieder en Verzoek"]
  end
    A["Aanbieder<br>(uitvoerend)"] --> B
    B@{ shape: terminal}
    A@{ shape: rounded}
    style s1 fill:#FFF9C4,stroke:#FFF9C4
```

## Use Case beschrijving

**Titel:** Raadplegen van VerzoekAanbieder en Verzoek door de Aanbieder<br/>
**Actoren:** Aanbieder vernoemd in VerzoekAanbieder

### Precondities:
- 
