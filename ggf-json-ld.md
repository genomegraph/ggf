# GGF JSON-LD

GFF: Extention to the GFA format for inclusion of genotype and annotation (draft)

## V-tag: Example

V id pos len alt

* id: S の ID
* pos: 0-based; inter coordinate

```text
S 2 ACGTCT
V 2 2 3 A     -> ACAT
V 2 2 3 AAA   -> ACAAAT
V 2 2 3 AAAAA -> ACAAAAAT
```

Comments:

* V 自身のIDを持つべきでは

## W-tag: Example

W id node position offset len genotype

* id: W 自身の ID
* node: S の ID (+: forward, -: reverse)
* pos: S の長さ（必要？）
* offset: S 上での genotype の対象とする領域の開始位置 (0-based; inter coordinate)
* len: S 上での genotype の対象とする領域の長さ
* genotype: V の組み合わせ

```text
S 2 ACGTGTAAACCCT
V 2 2 1 A
V 2 4 1 C
V 2 4 1 AG
W 1 2+ 13 0 13 AC -> ACATCTAAACCCT
W 2 2+ 13 0 13 GC -> ACGTCTAAACCCT
W 3 2+ 13 0 13 G[AG] -> ACGTAGTAAACCCT
```

Comments:
* V に自身の ID を導入して、W の genotype では ID,ID,ID のような組み合わせを使っては？
* genotype の [] 表記がわかりづらい
* node の +/- も他の行と比べると異質？

### JSON-LD alternative

```json
{ 
  "@context": "http://genomegraph.org/contexts/ggf.jsonld",
  "segment": [
    {
      "@id": 2,
      "sequence": "ACGTGTAAACCCT",
      "length": 13
    }
  ],
  "variation": [
    {
      "@id": 1,  // newly introduced
      "segment": 2,
      "offset": 2,  // renamed from pos
      "length": 1,
      "alternative": "A"
    },
    {
      "@id": 2,
      "segment": 2,
      "offset": 4,
      "length": 1,
      "alternative": "C"
    },
    {
      "@id": 3,
      "segment": 2,
      "offset": 4,
      "length": 1,
      "alternative": "AG"
    }
  ],
  "genotype": [
    {
      "@id": 1,
      "segment": 2,
      "strand": "+",  // removed segment length as segment has length
      "offset": 0,
      "range": 13,
      "haplotype": [1, 2]
    },
    {
      "@id": 2,
      "segment": 2,
      "strand": "+",
      "offset": 0,
      "range": 13,
      "haplotype": []
    }
  ]
}
```

## w-tag: Example

```text
W 1 1+ 100 5 100 ATTTC-*AT
[ {avg_repeat: 6} {min_repeat: 4} {max_repeat: 8} {interval:2} w 1 2+ 100 0 100 ATGTGT
w 1 3+ 100 0 100 TGTGT
w 1 2+ 100 0 100 ATGTGT
w 1 3+ 100 0 100 TGTGT
]
W 1 4+ 100 0 70 C-AT**AAG
```

### JSON-LD alternative

```json
TO BE CONSIDERED
```
