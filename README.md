# Tropykus — Consulta de posición (solo lectura)

Front-end de una sola página que consulta, en vivo y sin conectar billetera, tu posición en los contratos de [Tropykus](https://tropykus.com) sobre Rootstock (RSK) a través del endpoint RPC público de [Blockscout](https://rootstock.blockscout.com).

No firma ni puede firmar ninguna transacción. Solo lee datos on-chain con `eth_call`.

## Qué muestra

- **Depósitos**: balance en cada kToken (kDOC, kUSDRIF, kRBTC, kBPRO) convertido al activo real depositado.
- **Liquidez disponible para retiro**: `getCash()` de los mercados de DOC y USDRIF.
- **Préstamos activos**: deuda pendiente (`borrowBalanceStored`) en DOC y/o USDRIF.
- **Valores listos para copiar y pegar** en Blockscout, ya en formato wei (18 decimales), para las funciones `redeem`, `approve` y `repayBorrow` — sin necesidad de hacer conversiones manuales.

## Cómo usarlo

### Opción 1 — Abrirlo directo
Descargá `index.html` y abrilo con doble clic en cualquier navegador. No necesita servidor ni instalación.

### Opción 2 — GitHub Pages (para tener un link público)
1. Creá un repositorio nuevo en GitHub (podés llamarlo, por ejemplo, `tropykus-consulta`).
2. Subí este `index.html` a la raíz del repo (ver comandos más abajo).
3. En el repo: **Settings → Pages → Source**, elegí la rama `main` y la carpeta `/ (root)`.
4. A los pocos minutos vas a tener el sitio publicado en `https://<tu-usuario>.github.io/<nombre-del-repo>/`.

## Subir este proyecto a GitHub desde cero

```bash
# 1. Entrá a la carpeta donde tenés el archivo
cd ruta/a/la/carpeta

# 2. Inicializá el repo git
git init

# 3. Agregá el archivo
git add index.html README.md

# 4. Primer commit
git commit -m "Consulta de posición Tropykus vía Blockscout"

# 5. Creá el repo en GitHub (con GitHub CLI, opcional)
gh repo create tropykus-consulta --public --source=. --remote=origin

# — o si preferís hacerlo a mano sin gh CLI —
# Creá el repo vacío desde github.com y después:
git remote add origin https://github.com/<tu-usuario>/tropykus-consulta.git
git branch -M main

# 6. Subí los cambios
git push -u origin main
```

Cada vez que edites el archivo:

```bash
git add index.html
git commit -m "Descripción del cambio"
git push
```

## Cómo funciona técnicamente

Un solo archivo HTML, sin dependencias externas ni build step. Todo el JS y CSS está inline, y los logos de los tokens están embebidos como base64, así que no hace ninguna llamada externa salvo al endpoint RPC de Blockscout:

```
POST https://rootstock.blockscout.com/api/eth-rpc
```

Usa `eth_call` de solo lectura contra los siguientes contratos:

| Mercado | kToken |
|---|---|
| kUSDRIF | `0xDdf3CE45fcf080DF61ee61dac5Ddefef7ED4F46C` |
| kRBTC | `0x0aeadb9d4c6a80462a47e87e76e487fa8b9a37d7` |
| kDOC | `0x544eb90e766b405134b3b3f62b6b4c23fcd5fda2` |
| kBPRO | `0x405062731d8656af5950ef952be9fa110878036b` |

| Token subyacente | Contrato |
|---|---|
| DOC | `0xE700691Da7B9851F2F35f8b8182C69C53ccad9DB` |
| USDRIF | `0x3A15461d8AE0f0Fb5fA2629e9dA7D66A794a6E37` |

## Advertencia

Los montos que ofrece copiar para `approve`/`repayBorrow` usan `borrowBalanceStored`, que puede no incluir los intereses acumulados en los últimos bloques. Para el monto exacto de la deuda al momento de pagar, la guía original recomienda consultar `borrowBalanceCurrent` directamente en Blockscout antes de firmar.

Esta herramienta es un complemento de lectura, no reemplaza la verificación manual en Blockscout antes de firmar cualquier transacción.

## Licencia

MIT — usalo, modificalo y compartilo libremente.
