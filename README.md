# bgpnr — BGP Network Research Tool

Ferramenta de linha de comando para pesquisa e análise de ASNs (Autonomous System Numbers), combinando dados do **BGPView** e **PeeringDB**. Ideal para analistas de infraestrutura de redes e telecom.

---

## Recursos

| Funcionalidade | Fonte |
|---|---|
| Detalhes do ASN (nome, país, RIR, data de alocação, contatos) | BGPView |
| Informações de política de peering, IRR AS-SET | PeeringDB |
| Prefixos IPv4 e IPv6 com status RPKI (✅ Válido / ❌ Inválido) | BGPView |
| Upstreams — provedores de trânsito IPv4/IPv6 | BGPView |
| Downstreams — ASNs clientes IPv4/IPv6 | BGPView |
| Internet Exchanges com IPv4/IPv6, velocidade, RS Peer e BFD | PeeringDB + BGPView |
| Facilities / Data Centers / POPs | PeeringDB |
| Peers em IXs comuns | PeeringDB |
| Busca de ASNs por nome | BGPView + PeeringDB |

---

## Requisitos

- Python 3.6+
- Bibliotecas:

```bash
apt install python3-requests python3-tabulate
# ou
pip3 install requests tabulate
```

---

## Instalação

```bash
wget https://raw.githubusercontent.com/efilho89/BGP-CONSULTA/main/bgpnr -O /usr/local/bin/bgpnr
chmod +x /usr/local/bin/bgpnr
```

### Verificar instalação

```bash
bgpnr --help
```

---

## Uso

### Consulta completa de um ASN

```bash
bgpnr 15169
bgpnr AS15169      # aceita o formato AS+número
```

Exibe em sequência: detalhes, prefixos (RPKI), upstreams, downstreams, IXs, facilities e peers.

---

### Busca por nome

```bash
bgpnr -s Claro
bgpnr -s "vivo telecom"
```

Combina resultados do BGPView e PeeringDB, eliminando duplicatas.

---

### Consultas específicas

| Opção | Descrição | Exemplo |
|---|---|---|
| `-p` / `--prefixes` | Prefixos IPv4/IPv6 + status RPKI | `bgpnr -p 1916` |
| `-u` / `--upstreams` | Upstreams (trânsito) | `bgpnr -u 1916` |
| `-d` / `--downstreams` | Downstreams (clientes) | `bgpnr -d 1916` |
| `-i` / `--ixs` | Internet Exchanges | `bgpnr -i 1916` |
| `-f` / `--facilities` | Facilities / POPs (PeeringDB) | `bgpnr -f 1916` |
| `--peers` | Peers em IXs comuns (PeeringDB) | `bgpnr --peers 1916` |
| `-s` / `--search` | Busca por nome | `bgpnr -s "NET Virtua"` |

---

## Exemplos de saída

### `bgpnr 1916` — Detalhes do ASN

```
════════════════════════════════════════════════════════════════
  AS1916 — Detalhes
════════════════════════════════════════════════════════════════
╒══════════════════╤══════════════════════════════════╕
│ AS Number        │ AS1916                           │
│ Nome             │ Associação Rede Nacional...      │
│ País             │ BR                               │
│ RIR              │ LACNIC                           │
│ Alocado em       │ 10/02/1997                       │
│ IRR AS-SET       │ AS-RNP                           │
│ Política de peer │ Open                             │
│ Fonte PeeringDB  │ ✅                               │
╘══════════════════╧══════════════════════════════════╛
```

### `bgpnr -p 1916` — Prefixos com RPKI

```
╒══════╤══════════════════╤════════╤══════════════╕
│ RPKI │ Prefixo IPv4     │ Nome   │ Descrição    │
╞══════╪══════════════════╪════════╪══════════════╡
│ ✅   │ 200.130.0.0/16   │ RNP    │ ...          │
│ ❌   │ 200.132.0.0/16   │ RNP    │ ...          │
╘══════╧══════════════════╧════════╧══════════════╛
  Resumo: 12 prefixo(s) — 9 RPKI válido(s)
```

### `bgpnr -i 1916` — Internet Exchanges

```
╒════════════════╤══════╤══════════╤═══════════════╤═══════════╤══════════╤══════════╤═════╕
│ IX             │ País │ Cidade   │ IPv4          │ IPv6      │ Veloc.   │ RS Peer  │ BFD │
╞════════════════╪══════╪══════════╪═══════════════╪═══════════╪══════════╪══════════╪═════╡
│ IX.br (PTT SP) │ BR   │ São Paulo│ 187.16.216.x  │ 2001:...  │ 1.0 Gbps │ ✅       │ ❌  │
╘════════════════╧══════╧══════════╧═══════════════╧═══════════╧══════════╧══════════╧═════╛
```

---

## Fontes de dados

- [BGPView API](https://bgpview.docs.apiary.io/) — prefixos, upstreams, downstreams, IXs, busca
- [PeeringDB API](https://www.peeringdb.com/apidocs/) — detalhes de peering, facilities, RS Peer, BFD, IRR

Nenhuma autenticação é necessária para uso básico. O PeeringDB possui limites de requisição para usuários anônimos; para uso intensivo, crie uma [API Key](https://docs.peeringdb.com/howto/api_keys/) e adicione ao header `Authorization: Api-Key SEU_TOKEN`.

---

## Créditos

Desenvolvido por **Rudimar Remontti** com auxílio de IA.
Baseado no script original [bgprr](https://remontti.com.br/bgprr).
