# dev-zettelkasten

이 저장소는 **제텔카스텐(Zettelkasten)** 방법론을 기반으로 CS 기초 지식과 SW 엔지니어링 개념을 유기적으로 연결하고 확장하는 개인 지식 베이스입니다.

정보를 저장하는 것을 넘어, 학습한 내용을 내면화하고 프로젝트 경험과 연결하여 기술적 통찰을 기록합니다.

---
## 📂 Directory Structure

조니 데시멀(Johnny.Decimal) 시스템을 차용한 구조를 사용합니다.

- **`00_Inbox`** : 분류되지 않은 아이디어, 스크랩, 데일리 메모
- **`10_Seed`** : 학습 중인 미완성 메모, 파편화된 초안
- **`20_Knowledge`** : 정제된 CS 및 SW 엔지니어링 지식 (OS, Network, DB 등)
- **`30_Project`** : 프로젝트 기록 및 회고 (기술적 의사결정, 트러블슈팅, 포트폴리오 자산)
- **`80_Assets`** : 문서에 포함된 모든 시각 자료 (이미지, 다이어그램, PDF 등)
- **`90_System`** : 효율적인 작성을 위한 템플릿 및 Obsidian 설정

---
## 🚀 Workflow

1. **수집**: 모든 새로운 생각과 메모는 `00_Inbox`에서 시작합니다. 
2. **정리**: 내용이 정제되면 `20_Knowledge` 또는 `30_Project`로 이동시킵니다. 
3. **연결**: 관련 있는 개념은 `[[문서명]]`을 통해 유기적으로 연결합니다. 
4. **자산**: 이미지는 어디서 첨부하든 자동으로 `80_Assets`에 관리됩니다.

---
## 🛠 Commit Convention

일관성 있고 명확한 히스토리 관리를 위해 아래 규칙을 따릅니다.

| type     | description                 |
| :------- | :-------------------------- |
| `note`   | 새로운 지식/프로젝트 문서 추가 (첨부파일 포함) |
| `update` | 기존 문서 내용 보완, 수정 및 오타 교정     |
| `move`   | 파일 이동, 폴더 구조 변경 및 파일 삭제     |
| `docs`   | README 및 시스템 설정 파일 수정       |

**Example:**
- `note: OSI 7계층 핵심 요약 정리`
- `update: 프로젝트 A 트러블슈팅 세부 내용 보완`
- `move: 자바 기초 노트를 Lang 폴더로 이동`

---
## ⚙️ Setup

이 저장소의 지식 체계를 유지하기 위해 아래 도구와 설정을 권장합니다.

**Required Tools:**
* [Obsidian](https://obsidian.md/): 로컬 마크다운 기반 지식 관리 도구
* [Git](https://git-scm.com/): 버전 관리 및 원격 저장소 동기화

**Obsidian Settings:**
저장소 루트의 `.obsidian` 설정을 공유하지만, 아래 항목은 반드시 확인해 주세요.

* **Files & Links**
    * `Default location for new notes`: `00_Inbox` (모든 메모의 시작점)
    * `Default location for new attachments`: `80_Assets` (이미지 및 파일 관리)
    * `New link format`: `Relative path to file` (GitHub 이미지 렌더링 호환성)
* **Core Plugins**
    * `Templates`: 활성화 후 `Template folder location`을 `90_System`으로 지정

**Sync:**
별도의 플러그인을 사용하지 않으므로, 작업 후 아래 커맨드를 통해 수동으로 동기화합니다.
```bash
git add .
git commit -m "type: description"
git push origin main
```

---
Managed by [PGJeong](https://github.com/PGJeong)