# Исследование: Компилятор языка Python для процессоров архитектуры RISC-V

Дата: 28.09.2026. Тема: реализации Python (компиляторы/интерпретаторы) для RISC-V (riscv64/rv32), перспективные beta-проекты, научные статьи, стандарты.

---

## 1. Готовые (стабильные) реализации

| Проект | Ссылка | Описание |
|---|---|---|
| **CPython** | https://github.com/python/cpython | Референсная реализация Python. Порт riscv64-unknown-linux-gnu официально принят как **Tier 3** платформа по PEP 11 (август 2026, Python Insider: https://blog.python.org/2026/08/riscv-now-officially-supported/ ; PEP 11 issue: https://github.com/python/steering-council/issues/360). Требуется фикс-постинг билдботов для Tier 2. |
| **MicroPython** | https://github.com/micropython/micropython (~22k★) | Лёгкий Python для МК. Официальные порты: QEMU riscv (RV32IMC/RV64IMC, `ports/qemu`), Raspberry Pi Pico 2 (RISC-V ядра RP2350), ESP32-C3, GD32VF103. Есть native emitter и gchelper для RV32I/RV64. |
| **PyPy** | https://www.pypy.org | Альтернативная реализация с tracing-JIT (RPython → генерация C). Порта riscv64 в актуальном виде нет — уже долгие годы открытый запрос; потенциальная база для собственного JIT-бэкенда через RPython/LLVM. |
| **GCC / LLVM (инструментальная база)** | https://gcc.gnu.org , https://llvm.org | Стандартные компиляторы с полноценными бэкендами riscv64/rv32 (RVV 1.0 vector extension в LLVM). Используются для сборки CPython/MicroPython и как цель для Nuitka/Cython/Numba. |
| **Nuitka / Cython** | https://nuitka.net , https://cython.org | Транспиляторы Python→C; собираются любым riscv64-кросс-компилятором (см. https://stackoverflow.com/questions/62298737/). Практический путь AOT-компиляции Python-кода под RISC-V. |
| **GraalVM Native Image** | https://www.graalvm.org | Поддерживает RISC-V как цель AOT-компиляции (включая Python-подмножество GraalPy) — применимо для нативных бинарей на RISC-V. |

## 2. Перспективные / beta-проекты

| Проект | Ссылка | Статус |
|---|---|---|
| **PyTorch for RISC-V** | https://discuss.pytorch.org/t/risc-v-architecture-support-roadmap-for-pytorch/224278 | Официальный roadmap: RVV/RVM SIMD, ATen-операторы, нативный CI. Wheel-пакеты: https://pypi.org/project/pytorch-riscv64/ . |
| **wheel_builder / riscv64 PyPI wheels** | https://riseproject.dev/2025/05/14/easy-installation-of-binary-python-packages-on-riscv64-devices/ , https://github.com/pyca/cryptography/issues/14460 | Индекс PEP 503 с 50+ riscv64 wheel для ML/AI стека (numpy, scipy, cryptography…). manylinux-riscv64 на подходе. |
| **xDSL + MLIR → RISC-V** | https://arxiv.org/html/2603.17800v1 | Компиляция через MLIR с генерацией RISC-V vector кода; xDSL (Python-фреймворк для MLIR-диалектов) — перспективный путь «Python как компилятор для RISC-V». |
| **Pydgin for RISC-V** (статья: https://people.ece.cornell.edu/berkin/ilbeyi-pydgin-riscv2016.pdf) | Быстрый DSL-симулятор ISA на Python — основа для прототипирования компиляторов/расширений RISC-V. |
| **RGen** (CARRV 2023 paper: https://carrv.github.io/2023/papers/CARRV2023_paper_6_Tu.pdf) | Генератор компилятора, симулятора и ассемблера RISC-V из одного описания ISA — beta, активно развивается. |
| **CircuitPython/MicroPython RISC-V эмулятор на чистом Python** | https://www.adafruitdaily.com/2025/06/02/python-on-microcontrollers-newsletter-a-risc-v-emulator-that-runs-circuitpython-micropython-and-more-circuitpython-python-micropython-thepsf-raspberry_pi/ | RV32I эмулятор, запускающий MicroPython — образовательная база для собственных компиляторных экспериментов. |
| **CPython Tier 2 продвижение + buildbot riscv64** | https://docs.python.org/devguide/ | Текущая работа сообщества: билдботы на железе RISC-V, пакеты riscv64 в Debian/Fedora (https://wiki.debian.org/RISC-V). |

## 3. Статьи и публикации

| Публикация | Ссылка |
|---|---|
| «RegCPython: A Register-based Python Interpreter for Better Performance» (ACM TACO, 2023) — регистровая VM вместо стековой, прирост до 1.29×; идея применима к RISC-V бэкенду. | https://dl.acm.org/doi/10.1145/3568973 |
| «Python Interpreter Performance Deconstructed» (DYLA'14) — декомпозиция стоимости фич динамического языка. | https://www.cristal.univ-lille.fr/dyla14/papers/dyla14-8-Python_Interpreter_Performance_Deconstructed.pdf |
| «Compact Native Code Generation for Dynamic Languages on Micro-cores» (TECS 2021) — генерация нативного кода для RISC-V microcores, упоминание Numba-подхода. | https://arxiv.org/html/2102.02109v1 |
| «Enabling RISC-V Vector Code Generation in MLIR through xDSL» (arXiv 2603.17800, 2026) | https://arxiv.org/html/2603.17800v1 |
| «Vectorizing PyTorch for RISC-V RVV» (UPC/Sorbonne internship report) — векторизация ATen под RVV. | https://upcommons.upc.edu/bitstreams/62044513-c060-49e8-bc4b-993a7e2fb730/download |
| «Pydgin for RISC-V: A Fast and Productive Instruction-Set Simulator» (RISC-V Workshop 2016). | https://people.ece.cornell.edu/berkin/ilbeyi-pydgin-riscv2016.pdf |
| «RGen: A Tool for Generating RISC-V Compiler, Simulator, and Assembler» (CARRV 2023). | https://carrv.github.io/2023/papers/CARRV2023_paper_6_Tu.pdf |
| «Analyzing RISC-V Compiler Toolchain by Adopting Topic Modeling» (IEEE Access 2025). | https://ieeexplore.ieee.org/iel8/6287639/11323511/11415573.pdf |
| «Triton kernel performance on RISC-V CPU» (riscv.org blog) — компиляция Triton-ядер (Python DSL) под RISC-V. | https://riscv.org/blog/triton-kernel-performance-on-risc-v-cpu/ |
| «From CISC to RISC: Language-Model Guided Assembly Transpilation» (arXiv 2411.16341) — трансляция ассемблера x86→RISC-V. | https://arxiv.org/pdf/2411.16341 |

## 4. Общепринятые стандарты

| Стандарт | Ссылка | Значение для темы |
|---|---|---|
| **RVA23 / RVB23 Profiles** (ratified 2024) | https://docs.riscv.org/reference/rva23/v1.0/rva23-profiles.html | Целевая модель «универсального» RISC-V-ядра: RV64GC + обязательные расширения (Zbb и др.). Компиляторы Python-стека должны таргетироваться на RVA23U64. |
| **Unprivileged ISA Manual** (т. 1) | https://riscv.org/technical/specifications/ | Базовый ISA RV32/RV64, расширения M/A/F/D/C. |
| **RISC-V V Vector Extension 1.0** | https://docs.riscv.org/reference/isa/extensions/vector/_attachments/riscv-v-spec.pdf | Векторные инструкции (VLEN/ELEN/LMUL) — базис для SIMD-компиляции (PyTorch, Numba, MLIR). |
| **Privileged Architecture** (т. 2), SBI, ELF psABI (lp64d) | https://riscv.org/technical/specifications/ , https://github.com/riscv-non-isa/riscv-elf-psabi-doc | Системное окружение, ABI, вызовы — всё, что нужно для генерации объектного кода компилятором. |
| **PEP 11 (CPython platform tiers)** | https://peps.python.org/pep-0011/ | Политика уровней поддержки платформ CPython; Tier 3 для riscv64. |
| **manylinux / PEP 600, wheel tagging** | https://peps.python.org/pep-0600/ | Стандарт бинарных wheel; добавляется тег manylinux_riscv64. |
| **Debian riscv64 port** | https://wiki.debian.org/RISC-V | Эталонный дистрибутив-ориентир: тулчейн gcc-riscv64-linux-gnu, QEMU-эмуляция, FTBFS-политики. |

## Методология

Поиск вёлся через: `gh search repos` (GitHub CLI), firecrawl web/developer search, firecrawl_research (arXiv/Semantic Scholar индекс), официальные сайты riscv.org, python.org, IEEE/ACM. Все URL проверены по результатам выдачи.
