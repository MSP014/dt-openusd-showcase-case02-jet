# Omniverse MCP Helpers

Project-level helpers for querying local NVIDIA Omniverse MCP servers during
Case 02 OpenUSD and Kit application development:

- USD Code;
- Kit;
- OmniUI.

This is not a production dependency. It is a lightweight development/reference
tool for checking USD API details, Kit extension information, OmniUI examples,
and knowledge-base entries while building the Case 02 Omniverse runtime layer.

## Requirements

- Docker Desktop running.
- Local MCP servers as needed:
  - USD Code MCP on `localhost:9903`;
  - Kit MCP on `localhost:9902`;
  - OmniUI MCP on `localhost:9901`.
- Python 3.10+ through the project `case02-env` environment.
- `KIT_USD_AGENTS_ROOT` pointing to a local clone of
  `NVIDIA-Omniverse/kit-usd-agents`.
- Required credentials configured in the local environment used by
  `kit-usd-agents`.

## Start Servers

```powershell
Push-Location "$env:KIT_USD_AGENTS_ROOT\source\mcp"
docker compose --env-file .env -f docker-compose.ngc.yaml up -d usd-code-mcp kit-mcp omni-ui-mcp
Pop-Location
```

To stop the servers:

```powershell
Push-Location "$env:KIT_USD_AGENTS_ROOT\source\mcp"
docker compose -f docker-compose.ngc.yaml stop usd-code-mcp kit-mcp omni-ui-mcp
Pop-Location
```

Individual servers can be started or stopped separately when only one helper is
needed.

## Usage

USD Code MCP:

```powershell
conda run -n case02-env python tools/mcp/usd_mcp_client.py list-tools
conda run -n case02-env python tools/mcp/usd_mcp_client.py list-modules
conda run -n case02-env python tools/mcp/usd_mcp_client.py search-knowledge "USD variants"
conda run -n case02-env python tools/mcp/usd_mcp_client.py search-code "How to create a USD stage?"
conda run -n case02-env python tools/mcp/usd_mcp_client.py module-detail "Usd,UsdGeom"
conda run -n case02-env python tools/mcp/usd_mcp_client.py class-detail "UsdStage"
conda run -n case02-env python tools/mcp/usd_mcp_client.py method-detail "Open" --class-name "UsdStage"
```

Kit MCP:

```powershell
conda run -n case02-env python tools/mcp/kit_mcp_client.py list-tools
conda run -n case02-env python tools/mcp/kit_mcp_client.py search-knowledge "Kit extension lifecycle"
conda run -n case02-env python tools/mcp/kit_mcp_client.py search-app-templates "industrial digital twin"
conda run -n case02-env python tools/mcp/kit_mcp_client.py api-detail "omni.ui@Window"
```

OmniUI MCP:

```powershell
conda run -n case02-env python tools/mcp/omni_ui_mcp_client.py list-tools
conda run -n case02-env python tools/mcp/omni_ui_mcp_client.py list-classes
conda run -n case02-env python tools/mcp/omni_ui_mcp_client.py search-code "create a button with callback"
conda run -n case02-env python tools/mcp/omni_ui_mcp_client.py class-detail "Button"
```

## Repository Boundary

The Case 02 repository keeps only these small helper scripts and their
documentation. The following remain outside this repository:

- local `.env` files and API keys;
- local clones of `kit-usd-agents`;
- Docker volumes, images, and generated containers;
- wheels, build output, caches, and generated MCP artefacts.

This keeps the project repository reproducible while avoiding secrets,
generated dependencies, and local machine state.
