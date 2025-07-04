# WorldTranslationExtractor

Minecraft 세계에서 번역 가능한 텍스트를 추출하여 현지화를 돕는 도구입니다.

다른 언어로 이 파일 읽기: [ES](./language/es/README.md)

## 기본 정보

저장된 세계와 관련 파일 전체를 스캔하여 json 텍스트 컴포넌트(및 해당되는 경우 일반 텍스트)를 찾아 translate json 컴포넌트로 교체합니다.

현재 Minecraft Java Edition 1.21을 지원합니다. NBT 구조를 관리하는 코드는 모듈식으로 설계되어 있어, 다양한 Minecraft 버전에 맞는 추출기를 정의하고 동적으로 불러올 수 있습니다.

[Amulet world editor](https://www.amuletmc.com/) API에 의존합니다.

## 사용법
World Translation Extractor는 현재 커맨드라인 인터페이스(CLI)를 제공합니다.
```
python3 main.py [-h] --world WORLD [--out OUT] [--force | -f]
                   [--lang LANG] [--extract EXTRACT] [--dimension DIMENSION]
                   [--keepdup | -k] [--sort | -s]
                   [--indent INDENT] [--batch BATCH]
                   [--versionless | -v]
옵션:
  -h, --help            도움말 메시지를 표시하고 종료합니다.
  --world WORLD, -w WORLD
            대상 세계의 경로입니다.
  --out OUT, -o OUT     번역된 세계 복사본을 출력할 경로입니다. 기본값은 <WORLD>_wte입니다.
  --force, --no-force, -f
            추출 전에 <OUT>의 기존 내용을 삭제합니다.
  --lang LANG, -l LANG  번역 json을 출력할 경로입니다. 기본값은 wte_lang.json입니다.
  --extract EXTRACT, -e EXTRACT
            세계에서 실행할 추출기입니다. 여러 개 선택 가능하며, 지정하지 않으면 모든 추출기가 실행됩니다.
  --dimension DIMENSION, -d DIMENSION
            스캔할 차원입니다. 여러 개 선택 가능하며, 지정하지 않으면 모든 차원이 스캔됩니다.
  --keepdup, --no-keepdup, -k
            중복 번역 텍스트를 별도의 키로 유지합니다.
  --sort, --no-sort, -s
            출력 json을 알파벳순으로 정렬합니다.
  --indent INDENT, -i INDENT
            출력 json의 들여쓰기 공백 수입니다.
  --batch BATCH, -b BATCH
            세계를 반복 처리할 때 <BATCH> 청크마다 저장합니다.
  --versionless, --no-versionless, -v
            추출기 데이터 버전 불일치를 무시합니다.
```
예시: `python3 main.py -w "C:\Users\el sus\AppData\Roaming\.minecraft\saves\WTE_Test"` 명령어는 WTE_Test라는 세계에서 기본 설정으로 번역을 추출합니다.

세계는 원본과 같은 파일 경로에 새 폴더로 복사됩니다. 추출된 문자열과 번역 키가 연결된 json 파일이 작업 파일 경로에 생성됩니다. 이 파일은 리소스팩 lang 파일과 동일한 형식입니다.

## 설치 방법
[Python](https://www.python.org/downloads/) 3.9 이상(3.11 이상 권장)에서 실행됩니다. Windows 사용자는 Microsoft Store를 통해 Python을 설치하면 편리합니다.

이 저장소를 다운로드하거나 클론한 후, `python3 -m pip install -r requirements.txt` 명령어로 필요한 의존성을 설치하세요.

## 추출기(Extractors)
이 저장소에는 다음과 같은 추출기 모듈이 포함되어 있습니다. 모두 Minecraft 1.21에서 작동하며, 일부는 이전 DataVersion도 지원합니다. 자세한 내용은 각 소스 파일을 참고하세요.
### 타일 추출기
타일 추출기는 세계의 모든 타일 엔티티와 데이터팩 또는 generated 폴더 내 구조 파일에 있는 타일 엔티티를 처리합니다.
#### `bee`
`beehive`와 `bee_nest`를 관리합니다. `bees` 태그에서 엔티티를 추출합니다.
#### `command_block`
`command_block`을 관리합니다. `Command` 필드에서 컴포넌트를 추출합니다.
#### `container`
다음 컨테이너를 관리합니다: `'chest', 'furnace', 'shulker_box', 'barrel', 'smoker', 'blast_furnace', 'trapped_chest', 'hopper', 'dispenser', 'dropper', 'brewing_stand', 'campfire', 'chiseled_bookshelf', 'crafter'`. `CustomName`과 내부 `Items`를 추출합니다.
#### `item_tile`
`jukebox`의 `RecordItem`, `lectern`의 `Book`, `decorated_pot`의 `item`을 추출합니다.
#### `sign`
`sign`과 `hanging_sign`을 관리합니다. `front_text`와 `back_text`의 `messages`를 추출합니다.
#### `spawner`
`mob_spawner`를 관리합니다. `SpawnData`와 `SpawnPotentials`에서 엔티티를 추출합니다.
#### `trial_spawner`
`trial_spawner`를 관리합니다. `spawn_data`와 `spawn_potentials`의 `normal_config`, `ominous_config`에서 엔티티를 추출합니다.
#### `vault`
`vault`를 관리합니다. `key_item`, `display_item`, `items_to_eject`에서 아이템을 추출합니다.
### 엔티티 추출기
엔티티 추출기는 세계의 모든 엔티티와 데이터팩 또는 generated 폴더 내 구조 파일에 있는 엔티티를 처리합니다.
#### `command_block_minecart`
`command_block_minecart`를 관리합니다. `Command` 필드에서 컴포넌트를 추출합니다.
#### `container_entity`
`hopper_minecart`, `chest_minecart`, `chest_boat`, `trader_llama`, `mule`, `llama`, `donkey`의 `Items`와, `wandering_trader`, `villager`, `pillager`, `player`, `piglin`, `allay`의 `Inventory`를 추출합니다.
#### `entity`
모든 엔티티에 대해 실행됩니다. `CustomName`과 `Passengers`의 엔티티를 추출합니다.
#### `item_entity`
`arrow`, `spectral_arrow`, `ominous_item_spawner`, `trident`, `item_display`의 `item`; `item_frame`, `eye_of_ender`, `item`, `snowball`, `small_fireball`, `potion`, `fireball`, `experience_bottle`, `ender_pearl`, `egg`의 `Item`; `firework_rocket`의 `FireworksItem`; `zombie_horse`, `skeleton_horse`, `mule`, `horse`, `donkey`의 `SaddleItem`; `arrow`, `spectral_arrow`의 `weapon`을 추출합니다.
#### `mob`
모든 엔티티에 대해 실행됩니다(향후 확장 대비). `ArmorItems`, `HandItems`, `body_armor_item`의 아이템을 추출합니다.
#### `player`
`player`를 관리합니다. `EnterItems`의 아이템과 `ShoulderEntityLeft`, `ShoulderEntityRight`의 엔티티를 추출합니다.
#### `spawner_minecart`
`spawner_minecart`를 관리합니다. `SpawnData`와 `SpawnPotentials`에서 엔티티를 추출합니다.
#### `text_display`
`text_display`를 관리합니다. `text`를 추출합니다.
#### `villager`
`villager`, `zombie_villager`, `wandering_trader`를 관리합니다. `Offers`의 아이템을 추출합니다.
### 아이템 추출기
아이템 추출기는 엔티티, 블록 엔티티, 기타 아이템 내에서 발견된 모든 아이템을 처리합니다.
#### `book`
`written_book`을 관리합니다. `pages`와 `title`에서 텍스트를 추출합니다. 제목 번역은 다른 이름이 없을 경우 아이템의 커스텀 이름으로 설정됩니다. `author`는 생략됩니다. 이는 `title`과 `author`가 json 컴포넌트를 지원하지 않기 때문입니다.
#### `item`
모든 아이템에 대해 실행됩니다. `custom_name`, `item_name`, `lore`를 추출하며, `block_entity_data`의 타일, `container`의 아이템, `entity_data`의 엔티티도 추출합니다.
### 데이터 파일 추출기
데이터 파일 추출기는 저장 폴더 내 특정 dat 파일을 대상으로 합니다.
#### `level`
`level.dat`를 관리합니다. `Player`와 `CustomBossEvents`의 보스바 타이틀을 추출합니다.
#### `score`
`scoreboard.dat`를 관리합니다. `Objectives`의 스코어보드 목표 이름, `Teams`의 팀 이름, 접두사, 접미사를 추출합니다.
#### `storage`
모든 커맨드 저장 dat 파일을 관리합니다. 가능한 모든 컴포넌트를 재귀적으로 반복하며, json 텍스트 컴포넌트만 교체합니다.
### 텍스트 파일 추출기
텍스트 파일 추출기는 데이터팩에 포함된 일반 텍스트 파일을 대상으로 합니다.
#### `function`
모든 데이터팩 mcfunction 파일을 관리합니다. 발견된 텍스트 컴포넌트와 `bossbar` 명령어의 일반 텍스트를 추출합니다.
#### `json`
모든 데이터팩 json 파일을 관리합니다. 발견된 텍스트 컴포넌트를 추출합니다.

## 크레딧
이 도구를 사용하는 프로젝트에서 명시적 크레딧 표시는 필수가 아니지만, 해주시면 감사하겠습니다.

## 기여하기
새로운 추출기 추가나 Minecraft 최신 버전 대응 등 기여를 환영합니다.

이 도구 자체도 번역할 수 있습니다.(`language` 폴더를 통해서) 다른 언어로의 번역도 환영합니다!
