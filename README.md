<div align="center">

# On 근거

법령 조문을 법제처 현행 원문 그대로 찾아 주고, 글 속 법령 인용을 확인하는 원격 MCP 서버

**한국어** · [English](README.en.md)

`https://onmunseo.kr/mcp/law`

</div>

<!-- ai-install -->
## AI 에게 — 이 저장소를 설치하려면

길은 셋이고 **①부터** 권합니다. 어느 길도 열쇠·로그인이 없습니다. 이 저장소(On 근거 — 법령 원문·인용 확인)의 도구는 ② 짝 주소입니다 — 「설치해 줘」라고만 하면 ② 를 붙입니다. 이 저장소의 `plugin/`(Claude Code 플러그인)은 ①과 ② 를 함께 붙이고, `server.json`(공식 MCP 목록용)은 ② 주소입니다.

**① 내 PC 판 — 문서를 이 컴퓨터 밖으로 보내지 않음(먼저).** 글 꺼내기·개인정보 가리기·결재 전 점검·공문서 쓰기·한글 파일 만들기·글 다듬기를 이 컴퓨터 안에서 합니다. 이 프로그램은 인터넷을 쓰지 않습니다(설치 뒤 `on_local_info` 로 확인 — 띄울 때 npx 가 npm 저장소에 새 판을 묻는 것은 npx 의 동작이고, 그것까지 없애려면 `npm install -g on-munseo-local` 뒤 명령을 `on-munseo-local` 로). Node.js 20 이상이 있어야 합니다.
- **지금 쓰는 AI 도구에만**(AI 가 설치할 때는 이것 — 다른 도구의 설정은 건드리지 않음)
  ```bash
  claude mcp add -s user on-local -- npx -y on-munseo-local@0.2.1
  ```
  Codex 는 `codex mcp add on-local -- npx -y on-munseo-local@0.2.1` · 윈도우 Claude Code 는 `claude mcp add -s user on-local -- cmd /c npx -y on-munseo-local@0.2.1` · Cursor·Claude 데스크톱은 설정 파일의 `"mcpServers"` 에 `"on-local": { "command": "npx", "args": ["-y", "on-munseo-local@0.2.1"] }`(윈도우 `"on-local": { "command": "cmd", "args": ["/c", "npx", "-y", "on-munseo-local@0.2.1"] }`) · VS Code 는 `code --add-mcp '{"name":"on-local","command":"npx","args":["-y","on-munseo-local@0.2.1"]}'`
- **사람이 이 PC 의 여러 AI 도구에 한 번에** — Claude Code·Claude 데스크톱·Codex·Cursor·VS Code 를 찾아 모두 등록합니다(바꾸기 전에 백업, 이미 있으면 건너뜀, 미리 보기 `--dry-run`, 되돌리기 `--undo`, ②까지 `--pair`).
  ```bash
  npx -y on-munseo-local@0.2.1 setup --pair
  ```
- **판 고정** — 위 「지금 쓰는 AI 도구에만」 줄은 판 번호를 적어 붙입니다(`on-munseo-local@0.2.1`). 자동으로 새 판을 받지 않고, 사람이 올릴 때만 바뀝니다(npm 에 새 판이 올라와도 이미 붙인 판은 그대로 · 인터넷이 끊겨도 한 번 받은 판은 뜸). (`setup` 은 이 판(0.2.1)까지 등록 줄에 `@latest` 를 적습니다 — 판을 고정하려면 도구별 한 줄로 붙입니다) 새 판으로 올리기: 등록한 줄의 판 번호만 새 판으로 바꿔 다시 붙입니다(Claude Code 는 `claude mcp remove -s user on-local` 뒤 새 판 번호로 위 한 줄) · 바뀐 점: [CHANGELOG](https://github.com/on-munseo/on-local/blob/main/CHANGELOG.md)

**② 짝 주소(법령) — 도구에 넣은 낱말·글만 서버로.** 법령 원문·글 속 인용 확인(On 근거)은 서버에만 있습니다. 서버로 가는 것은 넣은 법령 이름·조문 번호·인용을 확인할 글이고, 파일을 보내는 도구는 없습니다. 인용을 확인할 글에 개인정보가 있으면 ① 의 `on_mask_text` 로 가린 뒤 넣습니다.
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law
  ```
- **Codex** — 설정 파일(`~/.codex/config.toml`)에 두 줄을 더합니다(이미 있으면 건너뜀). `codex mcp add --url` 은 쓰지 마세요 — 곧바로 브라우저 로그인 창을 띄웁니다.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-geungeo >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-geungeo"; then printf '\n[mcp_servers.on-geungeo]\nurl = "https://onmunseo.kr/mcp/law"\n' >> "$C/config.toml"; (cd / && codex mcp get on-geungeo >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-geungeo" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-geungeo)
  ```
  윈도우이거나 명령 줄 `codex` 가 없는 곳(IDE 확장만)은 그 파일 끝에 `[mcp_servers.on-geungeo]`·`url = "https://onmunseo.kr/mcp/law"` 두 줄을 손으로 더합니다(파일에 `mcp_servers = { … }` 한 줄 표가 이미 있으면 그 안에 `on-geungeo = { url = "https://onmunseo.kr/mcp/law" }` 를 더함).
- **Cursor** `"on-geungeo": { "url": "https://onmunseo.kr/mcp/law" }` · **VS Code** `code --add-mcp '{"name":"on-geungeo","type":"http","url":"https://onmunseo.kr/mcp/law"}'` · **Claude 데스크톱·claude.ai**(사람이 화면에서) 설정 → 커넥터 → 맞춤 커넥터 추가 → 이름 `On 근거`, 주소 `https://onmunseo.kr/mcp/law` → 「연결」

**③ 서버 판 — Node.js 없이 바로, 도구 전부(공개 문서용).** 문서를 서버(국내 데이터센터)로 보내 메모리에서만 처리하고 버립니다. 공개 문서·본인 문서에만 씁니다.
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-document https://onmunseo.kr/mcp
  ```
- **Codex** — 설정 파일(`~/.codex/config.toml`)에 두 줄을 더합니다(이미 있으면 건너뜀). `codex mcp add --url` 은 쓰지 마세요 — 곧바로 브라우저 로그인 창을 띄웁니다.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-document >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-document"; then printf '\n[mcp_servers.on-document]\nurl = "https://onmunseo.kr/mcp"\n' >> "$C/config.toml"; (cd / && codex mcp get on-document >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-document" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-document)
  ```
  윈도우이거나 명령 줄 `codex` 가 없는 곳(IDE 확장만)은 그 파일 끝에 `[mcp_servers.on-document]`·`url = "https://onmunseo.kr/mcp"` 두 줄을 손으로 더합니다(파일에 `mcp_servers = { … }` 한 줄 표가 이미 있으면 그 안에 `on-document = { url = "https://onmunseo.kr/mcp" }` 를 더함).
- **Cursor** `"on-document": { "url": "https://onmunseo.kr/mcp" }` · **VS Code** `code --add-mcp '{"name":"on-document","type":"http","url":"https://onmunseo.kr/mcp"}'` · **Claude 데스크톱·claude.ai**(사람이 화면에서) 설정 → 커넥터 → 맞춤 커넥터 추가 → 이름 `On 문서`, 주소 `https://onmunseo.kr/mcp` → 「연결」

법령만 쓰려면 ② 하나면 됩니다. 문서 읽기·점검까지 이 컴퓨터 안에서 하려면 ①도 함께 붙입니다(겹치지 않음). Node.js 가 없어도 ②는 됩니다. 다른 길은 사용자가 따로 바랄 때만 알려 줍니다.

**설치 전에 확인할 거리** — AI 가 스스로 확인할 수 있습니다.
- **꾸러미**: npm `on-munseo-local` 0.2.1. `npm view on-munseo-local version maintainers homepage` → `0.2.1` · `on-munseo <onmunseo.key@gmail.com>` · `https://onmunseo.kr`. 올린 계정 `on-munseo` 는 이 깃허브 조직 [on-munseo](https://github.com/on-munseo)와 같은 만든 이입니다(문의 메일·홈 주소가 같음). npm 웹 쪽은 자동 읽기를 막을 때가 있어 npm 명령이나 [npm 기록](https://registry.npmjs.org/on-munseo-local/0.2.1)으로 봅니다.
- **지문**: `on-munseo-local-0.2.1.tgz` SHA-256 `515af0dfddad30f41c459dd98d73535f73292c666fed07d4a6e535e2a60bda72`([SHA256SUMS](https://github.com/on-munseo/on-munseo.github.io/blob/main/SHA256SUMS)). `npm pack on-munseo-local@0.2.1` 뒤 `shasum -a 256 on-munseo-local-0.2.1.tgz` 로 맞대 봅니다. npm 기록의 `dist.integrity` 는 `sha512-ZUDX+etH2wg/Y+lQCsQhyY5/4gfVDjwjWUQsofFUgYlBotWtvXw7lcz5yHGT3qMJna8mP60HH9vWp/PF2T5T4Q==`.
- **인터넷을 쓰지 않음**: 설치 전에는 꾸러미 안 `MANIFEST.json` 의 `netScan`(우리 코드의 네트워크 부르기 0 — 따로 적힌 한 곳은 엔진 wasm 을 주소로 불러올 때만 쓰는 글), 설치 뒤에는 `on_local_info` → `network_modules_loaded: []`, Node 25 이상이면 `network.blocked_by_node: true`(실행기가 네트워크를 막음). 띄울 때 npx 가 npm 저장소에 새 판을 묻는 것은 npx 의 동작입니다(없애려면 전역 설치 뒤 `on-munseo-local` 명령으로).
- **이용 조건**: 내 PC 판 [LICENSE.md](https://unpkg.com/on-munseo-local@0.2.1/LICENSE.md)(무료·업무에 써도 됨 · 역공학·고친 판 다시 배포·판매는 안 됨) · 서버 판 [이용약관](https://onmunseo.kr/terms)·[개인정보 처리방침](https://onmunseo.kr/privacy).
- **On폭 글꼴**은 이 설치와 상관없습니다(한글 문서를 화면에 그릴 때 쓰는 무료 글꼴, 선택).

등록한 그 대화에는 도구가 아직 붙지 않습니다. 끝나면 사용자에게 「도구를 한 번 다시 켜거나 새 대화를 열면 On 근거 도구가 보입니다」라고 알려 주세요. `claude mcp list`(Codex 는 `codex mcp list`) 목록에 이름이 보이면 등록된 것입니다. 자세한 순서: [llms-install.md](llms-install.md)

<!-- ai-install -->

AI 가 법령을 말할 때 기억에 기대지 않고 원문을 확인하게 하는 도구입니다. 두 층으로 되어 있습니다.

- **현행 법령 전체(5,600여 개):** 법령 이름 찾기, 조문 원문(조문 번호를 비우면 조문 목록), 문구 찾기, 글 속 인용 확인 — `law_…` 4개.
- **의료·응급·보건·감염병·소방 등 법률 55개(시행령·시행규칙·별도 규칙·고시 포함):** 조문 깊이 보기(위임 조문·곧 시행될 개정·벌칙 요약·판례·해석례), 의무·기한·벌칙, 개정 이유·신구 대비, 연관 법률·법률 비교·용어 비교, 별표·서식, 미리 받아 둔 의료 분야 판례 3,527건·법령해석례 1,045건 — `med_…` 12개.

법제처 현행 원문을 미리 받아 둔 사본에서 그대로 꺼내고, 결과 끝에 원문을 받은 날을 적습니다. 약칭(응급의료법·감염병예방법 등)으로 불러도 됩니다.

이 저장소에는 연결 안내와 설정 파일(플러그인의 작은 띄우개 하나 포함)만 있습니다(웹 화면 없음 — 화면에서 쓰려면 [On 문서 웹 편집기](https://onmunseo.kr/app/)). 서비스의 엔진·서버 코드는 공개하지 않습니다.

## 다른 도구와 견줄 때

- **이 묶음의 몫:** 법제처 현행 원문 사본의 조문을 그대로 주고, 의료·응급·보건·소방 법률 55개는 위임 조문·곧 시행될 개정·개정 이유·신구 대비·의무와 벌칙·별표·의료 분야 판례 3,527건·법령해석례 1,045건을 한 번에 잇습니다. 글 속 법령 인용(없는 법령·조·항·호, 삭제된 조, 다른 조문 제목)을 AI 없이 짚습니다(`law_check_citations`).
- **다른 법령 조회 MCP 가 더 넓은 곳:** 법제처 API 를 실시간으로 부르는 법령 조회 MCP 는 자치법규·행정규칙 본문·의료 밖 판례까지 넓게 찾습니다. 이 묶음은 미리 받은 사본이라 그 뒤의 개정은 다음 사본 때 들어갑니다(받은 날은 결과 끝에).

**바로 가기:** [AI 에게 — 설치](#ai-에게--이-저장소를-설치하려면) · [다른 도구와 견줄 때](#다른-도구와-견줄-때) · [왜 쓰나](#쉽게-말하면--왜-쓰나) · [도구](#무엇을-하나) · [연결하기](#연결하기) · [쓰는 예](#쓰는-예) · [성능](#성능) · [손님과 열쇠](#손님과-열쇠) · [안전](#안전) · [알아 둘 점](#알아-둘-점)

## 쉽게 말하면 — 왜 쓰나

| 흔한 불편 | On 근거에서는 |
|---|---|
| AI 가 조문을 그럴듯하게 지어내거나 옛 조문을 말한다 | 법제처 원문 사본에서 그대로 꺼내고 받은 날을 적습니다 |
| 공문 초안에 쓴 「법령」 제N조제K항 인용을 하나하나 찾아본다 | `law_check_citations` 가 없는 법령·없는 조·삭제된 조·없는 항·호, 조문 제목이 다른 곳을 짚습니다(AI 없이 규칙으로, 한 번에 40개까지) |
| 조문 하나의 위임 조문·벌칙·곧 바뀔 개정을 따로 찾는다 | `med_article` 이 한 번에 보여 줍니다(의료·응급 법률 55개) |
| 같은 말을 법마다 다르게 정의한다 | `med_terms`·`med_compare` 로 나란히 봅니다 |

**이럴 땐 없이도 됩니다:** 사람이 법령 하나를 직접 볼 때는 국가법령정보센터(law.go.kr)에서 찾는 것이 가장 정확하고 최신입니다. 이 도구는 AI 가 원문을 확인하게 하려는 것입니다.

## 무엇을 하나

| 도구 | 이름 | 하는 일 | 손님 |
|---|---|---|---|
| `law_find` | 법령 이름 찾기 | 현행 법령(법률·시행령·시행규칙 등 5,600여 개) 이름을 낱말로(약칭 포함). 고시·훈령 같은 행정규칙은 이름만 | ○ |
| `law_article` | 조문 원문 | 현행 법령 전체에서 조문 원문 그대로. 조문 번호를 비우면 조문 제목 목록 | ○ |
| `law_search` | 문구 찾기 | 법령을 주면 그 법령 본문에서(띄어쓰기 무시), 비우면 전체 법령 이름·조문 제목에서 | ○ |
| `law_check_citations` | 인용 확인 | 글 속 법령 인용을 현행 원문과 맞대어 없는 법령·조·항·호, 삭제된 조, 제목 다름을 짚음(한 번에 40개까지) | ○ |
| `med_laws` | 법률 목록 | 다루는 의료·응급 법률을 분야별로(약칭 포함) | ○ |
| `med_article` | 조문 깊이 보기 | 조문 원문 + 곧 시행될 개정 + 위임 조문 + 어기면(벌칙 요약) + 인용한 판례·해석례 | ○ |
| `med_search` | 문구 찾기 | 목록의 법률·시행령·시행규칙·별도 규칙 원문에서 | ○ |
| `med_duties` | 의무·기한·벌칙 | 한 조문의 의무·금지·기한 문장과 벌칙·과태료 조문, 처분 기준 별표 줄 | ○ |
| `med_changes` | 개정 이유·신구 대비 | 최근 1년·시행 예정 개정, 개정이유(법제처 원문), 바뀐 글자 【 】 | ○ |
| `med_upcoming` | 곧 시행될 개정 | 공포됐지만 아직 시행 전인 개정을 시행일 순으로 | ○ |
| `med_related` | 연관 법률 | 법률끼리의 인용 관계, 조문을 주면 그 조문을 가져다 쓰는 다른 법 조문까지 | ○ |
| `med_compare` | 법률 비교 | 같은 주제를 2~4개 법률에서 나란히 | ○ |
| `med_terms` | 용어 비교 | 같은 용어를 법마다 어떻게 정의했는지 | ○ |
| `med_checklist` | 의무 체크리스트 | 「누가」(법령에 쓰인 말 그대로)가 해야 할 일·하면 안 되는 일을 조문 줄로 | ○ |
| `med_annex` | 별표·서식 | 별표·서식 목록과 본문, 낱말 둘레만도 | ○ |
| `med_decisions` | 판례·법령해석례 | 미리 받아 둔 의료 분야 판례·해석례에서 찾기, id 를 주면 본문 | ○ |

- 손님에게 보이는 도구 16개는 2026-11-26 운영 서버에 손님으로 `tools/list` 를 불러 확인했습니다.
- 열쇠 권한과 상관없이 모든 열쇠와 손님이 씁니다. On 문서 전부 주소(`/mcp`)에도 같은 16개가 들어 있습니다.

## 연결하기

맨 위 [AI 에게 — 설치](#ai-에게--이-저장소를-설치하려면)의 세 길(① 내 PC 판 · ② 짝 주소 · ③ 서버 판) 가운데, 아래는 ② 짝 주소(이 묶음 — 법령)를 도구마다 붙이는 자세한 법입니다.

**Claude Code** — 터미널에 한 줄(`-s user` 는 모든 폴더에서 쓰기):

```bash
claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law
```

**Claude Code 플러그인** — ① 내 PC 판 + ② 법령 짝을 붙입니다(Node.js 20 이상, 윈도우도 그대로 — 작은 띄우개 `plugin/run-local.mjs` 가 npx 를 띄움). 제품 플러그인을 여럿 깔면 내 PC 판이 여럿 떠 같은 도구가 겹쳐 보이니 하나만 까세요. 한 줄(Claude Code 2.1.275 이상):

```bash
claude plugin install on-geungeo --marketplace on-munseo/on-geungeo
```

어느 앱이든 등록한 그 대화에는 도구가 아직 붙지 않습니다. 앱을 한 번 다시 켜거나(Claude Code 터미널은 `/exit` 뒤 `claude`, VS Code 는 새 대화) 새 대화를 열면 그다음부터 씁니다. 열쇠가 없으면 로그인 단계는 건너뛰고 손님으로 씁니다.

> 문서 도구 전부와 법령 원문(On 근거)을 함께 쓰려면 이 묶음 대신 `https://onmunseo.kr/mcp` 하나를 붙이세요(권장 — [On 문서](https://github.com/on-munseo/on-munseo.github.io)). 둘 다 붙이면 같은 도구가 두 번 보입니다.

### Claude Code — 더 보기

- 열쇠가 있으면 Claude Code 안에서 `/mcp` → `on-geungeo` → 인증(Authenticate)을 누르면 On 문서 로그인 창이 뜹니다. 열쇠를 넣으면 끝입니다.
- 머리말로 넣으려면 `claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law --header "Authorization: Bearer <열쇠>"` 처럼 붙입니다. 이렇게 하면 열쇠가 설정 파일(`~/.claude.json`)에 남으니 로그인 창 쪽을 권합니다.
- 플러그인을 두 줄로 붙이려면(옛 판): `claude plugin marketplace add on-munseo/on-geungeo` 다음 `claude plugin install on-geungeo@on-geungeo`. Claude Code 대화창에서는 `/plugin marketplace add on-munseo/on-geungeo` → `/plugin install on-geungeo@on-geungeo`.
- 확인 `claude mcp list` · 빼기 `claude mcp remove on-geungeo`.

### claude.ai · Claude 데스크톱(커넥터)

설정 → 커넥터 → 「사용자 지정 커넥터 추가」(화면에 따라 「맞춤 커넥터 추가」)에서 이름 `On 근거`, 주소 `https://onmunseo.kr/mcp/law` 를 넣고 「연결」을 누릅니다. 열쇠가 있으면 뜨는 On 문서 로그인 창에 넣고, 없으면 손님으로 씁니다. 요금제·판에 따라 메뉴 이름이 다르거나 이 메뉴가 없을 수 있습니다.

Claude 데스크톱을 설정 파일로 붙이려면 중계 도구 `mcp-remote` 를 씁니다(Node.js 필요). 맥 `~/Library/Application Support/Claude/claude_desktop_config.json` · 윈도우 `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "on-geungeo": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://onmunseo.kr/mcp/law"
      ]
    }
  }
}
```

열쇠를 머리말로 넣으려면 `"args"` 끝에 `"--header", "Authorization:${ON_AUTH}"` 를 더하고 `"env": { "ON_AUTH": "Bearer <열쇠>" }` 를 둡니다(쌍점 뒤를 띄우지 않는 것은 윈도우에서 인자가 갈라지지 않게 하려는 것). 저장한 뒤 Claude 데스크톱을 다시 켭니다.

### 챗GPT

1. 설정 → 앱·커넥터 → 고급 설정에서 「개발자 모드」를 켭니다(화면 판에 따라 다를 수 있음).
2. 「만들기」(사용자 MCP 서버 추가)에서 이름 `On 근거`, 주소 `https://onmunseo.kr/mcp/law`, 인증은 열쇠가 있으면 「OAuth」, 없으면 「없음」을 고릅니다.
3. 연결할 때 뜨는 On 문서 로그인 창에 열쇠를 넣고, 대화 창의 도구(+) 메뉴에서 켭니다.

- 챗GPT 에는 열쇠 대신 따로 만든 출입증이 갑니다(권한·하루 몫은 열쇠와 같음). 열쇠를 끊으면 출입증도 바로 막힙니다.

### Codex

열쇠 없이는 `~/.codex/config.toml` 에 두 줄을 넣습니다:

```toml
[mcp_servers.on-geungeo]
url = "https://onmunseo.kr/mcp/law"
```

- 열쇠가 없으면 `codex mcp add --url` 은 쓰지 마세요 — 곧바로 On 문서 로그인 창(브라우저)을 엽니다. 위 두 줄을 터미널 한 줄로 더하려면(이미 있으면 건너뜀):

  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-geungeo >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-geungeo"; then printf '\n[mcp_servers.on-geungeo]\nurl = "https://onmunseo.kr/mcp/law"\n' >> "$C/config.toml"; (cd / && codex mcp get on-geungeo >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-geungeo" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-geungeo)
  ```

- 열쇠가 있으면 위 두 줄을 넣은 뒤 로그인합니다: `codex mcp login on-geungeo`

### Cursor

`~/.cursor/mcp.json`(모든 프로젝트) 또는 프로젝트의 `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "on-geungeo": {
      "url": "https://onmunseo.kr/mcp/law"
    }
  }
}
```

Cursor 가 원격 MCP 로그인(OAuth)을 하지 못하는 판이면 `"headers": { "Authorization": "Bearer <열쇠>" }` 를 더합니다(열쇠가 파일에 남습니다).

### VS Code

작업 공간의 `.vscode/mcp.json`:

```json
{
  "servers": {
    "on-geungeo": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/law"
    }
  }
}
```

열쇠를 머리말로 넣되 파일에 적지 않으려면, 처음 켤 때 묻게 합니다:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "on-key",
      "description": "On 문서 열쇠",
      "password": true
    }
  ],
  "servers": {
    "on-geungeo": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/law",
      "headers": {
        "Authorization": "Bearer ${input:on-key}"
      }
    }
  }
}
```

### 직접 불러 보기(curl)

도구 목록(열쇠가 있으면 `-H "Authorization: Bearer <열쇠>"` 를 더함):

```bash
curl -s https://onmunseo.kr/mcp/law \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

`law_article` 부르기 — 글에 따옴표·줄바꿈이 많으면 이렇게 파일로 보내면 깨지지 않습니다:

```bash
cat > call.json <<'EOF'
{"jsonrpc": "2.0", "id": 1, "method": "tools/call", "params": {"name": "law_article", "arguments": {"law": "응급의료법", "jo": "제2조"}}}
EOF
curl -s https://onmunseo.kr/mcp/law \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  --data-binary @call.json
```

「응급의료에 관한 법률」 제2조(정의) 원문과, 끝 줄 「법제처 현행 법령(2026.11.14. 받음) 그대로 · 참고용」을 받았습니다(2026-11-26, 손님). 결과 `result.content` 의 글이 답입니다. 서버는 상태를 두지 않는 방식(Streamable HTTP, POST)이라 `initialize` 없이 `tools/call` 하나로 됩니다. 브라우저로 주소를 열면 안내 쪽이 보입니다.

## 쓰는 예

| 이렇게 말하면 | 이 도구가 |
|---|---|
| 「응급의료법 제2조 원문 보여 줘」 | `med_article` 또는 `law_article` |
| 「이 공문 초안에 쓴 법령 인용이 맞는지 확인해 줘」 | `law_check_citations` |
| 「감염병예방법에서 신고 의무와 어기면 받는 벌칙을 알려 줘」 | `med_duties` |
| 「곧 시행되는 응급의료법 개정이 뭐야?」 | `med_upcoming` · `med_changes` |
| 「구급차 규정을 응급의료법과 119구조구급법에서 나란히 보여 줘」 | `med_compare` |
| 「개인정보 보호법 시행령에서 「가명정보」가 나오는 조문을 찾아 줘」 | `law_search` |

AI 가 어느 도구를 부를지는 AI 가 정합니다. 위는 흔히 불리는 예입니다.

## 성능

법령 원문을 그대로 돌려주는 도구라 정확도 점수는 따로 내지 않습니다. 대신 담은 자료의 규모와 받은 날을 적습니다.

| 자료 | 규모 | 받은 날(2026-11-26 에 결과로 확인) |
|---|---|---|
| 현행 법령 전체(법률·시행령·시행규칙 등) | 5,600여 개 | 2026.11.14.(`law_article` 결과 끝 줄) |
| 의료·응급 법률(시행령·시행규칙·별도 규칙·고시 포함) | 55개 | 2026.11.23.(`med_article` 결과 끝 줄) |
| 의료 분야 판례 · 법령해석례 | 3,527건 · 1,045건 | 미리 받아 둔 사본 |

약한 곳: 미리 받아 둔 사본이라 그 뒤의 개정은 다음 사본 때 들어갑니다(받은 날은 결과 끝에 적힘). 고시·훈령 같은 행정규칙은 이름만 찾고, 본문은 의료·응급 법률에 딸린 것만 있습니다. 판례·해석례는 미리 받아 둔 의료 분야 것에서만 찾습니다. 결과는 참고용이며 최종 판단은 법령 원문과 담당 기관 해석으로 합니다.

## 손님과 열쇠

|  | 손님(열쇠 없음) | 열쇠 |
|---|---|---|
| 붙이는 법 | 인증 없이 주소만 붙임 | 연결할 때 뜨는 On 문서 로그인 창(OAuth)에 열쇠, 또는 `Authorization: Bearer <열쇠>` 머리말 |
| 이 주소의 도구 | 16개 | 16개 |
| 하루 몫 | 같은 인터넷 주소(IP)에서 하루 50번·1분에 6번(법령 도구 `law_`·`med_` 는 따로 하루 100번·1분에 20번) — On 문서의 모든 주소와 웹 화면을 합쳐 셈. 손님 전체의 하루 몫이 차면 한국 시간 자정까지 닫힘 | 열쇠마다 정해진 하루 몫과 동시에 처리하는 수 |
| 열쇠가 있어야 하는 것 | — | 없음(모든 열쇠와 손님이 같은 16개를 씀) |

- 열쇠 신청: `onmunseo.key [at] gmail.com`(제목에 「On 문서 열쇠 신청」). 쓰려는 곳(개인·업무·연구 등)과 필요한 묶음(예: On 근거)을 적어 주세요.
- 틀린 열쇠·끊긴 열쇠는 **401**, 열쇠에 없는 묶음은 **403** 입니다. 짧은 사이에 몰아 부르면 「너무 자주 요청」(429)이 나니 1분쯤 뒤 다시 부릅니다.
- 열쇠를 주소 뒤에 붙이지 마세요. 대화 기록과 접속 기록에 남습니다.

## 안전

1. **남기지 않습니다.** 보낸 글은 처리가 끝나면 버리고, 창구 기록에는 열쇠 별칭·도구 이름·보낸 크기·걸린 시간·성공 여부만 남깁니다.
2. **원문을 그대로 돌려줍니다.** 조문·판례·해석례는 미리 받아 둔 사본에서 꺼내고 AI 가 고쳐 쓰지 않습니다. 인용 확인은 규칙으로 합니다.
3. **공개 자료만 돌려줍니다.** 돌려주는 것은 법제처 국가법령정보센터의 공개 자료(법령·판례·법령해석례)뿐입니다.
4. **대량 수집을 막습니다.** 결과를 긁어 모으려는 호출은 하루 동안 멈춥니다(아래 「알아 둘 점」).

보안 문제는 [SECURITY.md](SECURITY.md)에 적힌 곳으로 알려 주세요.

## 알아 둘 점

- **무료 시범 서비스입니다.** 기능·하루 몫·주소는 바뀌거나 멈출 수 있습니다. 바뀌면 [CHANGELOG.md](CHANGELOG.md)에 적습니다.
- **결과는 참고용입니다.** 결재 전에 사람이 확인해 주세요.
- 결과가 12,000자를 넘으면 뒤를 자르고, 범위를 좁혀 다시 물어 달라고 알립니다.
- 짧은 시간에 아주 많이 부르거나 한 법의 조문을 차례로 훑으면 그 열쇠(손님은 그 주소)의 On 근거 도구를 하루 동안 멈춥니다. 한 번에 2건씩 처리하고, 몰리면 차례를 기다립니다.
- On 근거 도구도 같은 하루 몫으로 셉니다.
- 인터넷으로만 씁니다. 업무망(폐쇄망)에서는 쓸 수 없습니다.
- 앱마다 메뉴 이름과 판이 달라 화면이 위 설명과 다를 수 있습니다. 안 되면 [SUPPORT.md](SUPPORT.md)를 봐 주세요.

## 이용 조건·개인정보·고지

- 결과를 대량으로 모으거나 다른 AI 모델 학습에 쓰는 것은 금지합니다. 자동화로 남용하면(한도를 넘기려 열쇠 여러 개 쓰기, 쉬지 않고 계속 부르기, 결과 긁어 모으기 등) 미리 알리지 않고 열쇠를 끊습니다. 엔진·규칙·코드를 꺼내 가거나 거꾸로 풀어내는 것은 허락하지 않습니다.
- 요약은 [MCP 안내의 이용 조건](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#5-이용-조건약관-요약), 전문은 [이용약관](https://onmunseo.kr/terms)·[개인정보 처리방침](https://onmunseo.kr/privacy)에 있습니다.
- On 근거가 돌려주는 법령·판례·법령해석례 원문은 법제처 국가법령정보센터의 공개 자료입니다.
- Claude·ChatGPT·Cursor·VS Code 등 이름은 각 권리자의 것이며 연결 방법을 알리려고만 씁니다. 고지 전문은 [MCP 안내의 고지](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#고지)에 있습니다.

## 말 풀이

- **MCP** — AI 도구가 바깥 도구를 부르는 공개 규약(Model Context Protocol). 이 주소를 MCP 서버로 붙이면 AI 가 대화 안에서 이 도구들을 부릅니다.
- **원격 MCP 서버** — 내 컴퓨터에 깔지 않고 인터넷 주소로 붙는 MCP 서버. 설치할 프로그램이 없습니다.
- **묶음 주소** — On 문서의 기능 하나만 담은 주소(`/mcp/text` 등). 전부는 `/mcp` 입니다.
- **열쇠** — On 문서가 주는 사용 권한 값. 하루 몫과 쓸 수 있는 묶음이 정해져 있습니다. 남에게 보이면 안 됩니다.
- **손님** — 열쇠 없이 쓰는 것. 도구 일부와 작은 하루 몫으로 바로 씁니다.
- **OAuth(로그인 창)** — 연결할 때 뜨는 On 문서 로그인 창에 열쇠를 넣는 방식. 앱에는 열쇠 대신 따로 만든 출입증이 갑니다.
- **위임 조문** — 법률이 「대통령령으로 정한다」처럼 아래 법령에 맡긴 내용을 실제로 정한 시행령·시행규칙 조문.
- **신구 대비** — 개정 전과 뒤의 조문을 나란히 놓고 바뀐 글자를 표시한 것.
- **법령해석례** — 법제처가 법령의 뜻을 묻는 질의에 답한 해석.
- **행정규칙** — 고시·훈령·예규처럼 행정 기관이 정한 규칙.

## On 문서의 다른 묶음

| 제품 | 저장소 | 주소(앞에 `https://onmunseo.kr`) |
|---|---|---|
| **On 문서(전부)** | [on-munseo.github.io](https://github.com/on-munseo/on-munseo.github.io) — 웹 편집기와 MCP 전부 | `/mcp` |
| On 글꺼내기 | [on-text](https://github.com/on-munseo/on-text) | `/mcp/text` |
| On 결재점검 | [on-check](https://github.com/on-munseo/on-check) | `/mcp/check` |
| On 개인정보 가리기 | [on-mask](https://github.com/on-munseo/on-mask) | `/mcp/mask` |
| On HWPX 만들기 | [on-hwpx](https://github.com/on-munseo/on-hwpx) | `/mcp/hwpx` |
| On 공문서 쓰기 | [on-gov](https://github.com/on-munseo/on-gov) | `/mcp/gov` |
| On 글 다듬기 | [on-tidy](https://github.com/on-munseo/on-tidy) | `/mcp/tidy` |
| On 근거 | 이 저장소 | `/mcp/law` |
| On폭 글꼴 | [napurrin/onpok-fonts](https://github.com/napurrin/onpok-fonts) — 무료 글꼴(SIL OFL 1.1) · MCP 설치와 상관없음(선택) | — |

## 라이선스

글은 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.ko), 설정 파일(`server.json`·`.claude-plugin/`·`plugin/`)은 MIT 입니다. 「On 문서」·「On 근거」 이름과 로고는 이 허락에 들어가지 않습니다. 서비스의 엔진·서버 코드는 이 저장소에 없고 공개하지 않습니다. 전문은 [LICENSE](LICENSE)에 있습니다.

---

만든 사람: [napurrin](https://github.com/napurrin) — 코코에게 배우면서 만들고 고칩니다.
