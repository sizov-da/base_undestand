<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

### Как смотреть процесс распаковки архива tar

**Стандартная команда**

```bash
tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1
```

по умолчанию не показывает прогресс. Есть несколько способов отслеживать процесс:

#### 1. Вариант с подробным выводом файлов

Добавьте флаг `-v` (verbose), чтобы видеть имена файлов по мере распаковки:

```bash
tar -xzvf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1
```

Это не даст процентного прогресса, но покажет, какие файлы уже извлечены[^1][^2].

#### 2. Использование утилиты pv для отображения прогресса

Если архив очень большой, можно использовать утилиту `pv` (pipe viewer), чтобы видеть скорость и примерный прогресс:

```bash
pv ./node-snapshot/FullNode_output-directory.tgz | tar -xzv -C ./nods/data/tron/ --strip-components=1 -f -
```

- `pv` покажет прогресс-бар, скорость и примерное время.
- Для более точного процента можно указать размер архива через `-s`, например:

```bash
pv -s $(stat -c %s ./node-snapshot/FullNode_output-directory.tgz) ./node-snapshot/FullNode_output-directory.tgz | tar -xzv -C ./nods/data/tron/ --strip-components=1 -f -
```

- Если `pv` не установлен, его можно поставить через `sudo apt install pv`[^3][^4][^5].


#### 3. Использование checkpoint в tar

Можно добавить опцию `--checkpoint=1000 --checkpoint-action=dot`, чтобы каждые 1000 файлов выводилась точка:

```bash
tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1 --checkpoint=1000 --checkpoint-action=dot
```

Это удобно для архивов с большим количеством файлов[^6].

### Как запустить процесс распаковки в фоне

#### 1. Использование `&` для фонового запуска

Добавьте `&` в конце команды:

```bash
tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1 &
```

- После запуска можно посмотреть PID процесса через `jobs` или `ps`.


#### 2. Использование `nohup` для работы после выхода из сессии

```bash
nohup tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1 > tar.log 2>&1 &
```

- Вывод будет сохраняться в файл `tar.log`.
- Процесс продолжит работать даже после выхода из терминала[^7].


#### 3. Использование `screen` или `tmux`

- Запустите сессию `screen` или `tmux`, выполните команду, и можно отсоединиться от сессии, не прерывая процесс.


### Итоги

- Для отслеживания процесса используйте флаг `-v`, утилиту `pv` или опции `--checkpoint`.
- Для фонового запуска используйте `&`, `nohup` или терминальные мультиплексоры (`screen`, `tmux`).
- Для больших архивов рекомендуется сохранять вывод в лог-файл для последующего анализа.

Эти методы позволяют удобно контролировать и не терять процесс даже при долгой распаковке больших снапшотов[^1][^3][^7].

<div style="text-align: center">⁂</div>

[^1]: https://superuser.com/questions/168749/is-there-a-way-to-see-any-tar-progress-per-file

[^2]: https://linuxize.com/post/how-to-create-and-extract-archives-using-the-tar-command-in-linux/

[^3]: https://www.howtogeek.com/428654/how-to-monitor-the-progress-of-linux-commands-with-pv-and-progress/

[^4]: https://gist.github.com/armancohan/574d0a2b55090bfa917c7fec76150f0a

[^5]: https://gist.github.com/Kautenja/af670104f13c94f92b69ce054ee96b42

[^6]: https://www.youtube.com/watch?v=xIfsdW6ripo

[^7]: https://forum.directadmin.com/threads/extracting-tar-file-in-background-server-particular-directory.28964/

[^8]: docker-compose.yml

[^9]: http://www.gnu.org/software/tar/manual/html_section/verbose.html

[^10]: https://askubuntu.com/questions/1316098/progress-bar-when-doing-a-tar

[^11]: https://stackoverflow.com/questions/19372373/how-to-add-progress-bar-to-a-somearchive-tar-xz-extract

[^12]: https://unix.stackexchange.com/questions/677910/how-can-i-see-any-tar-progress-when-i-use-xform

[^13]: https://www.reddit.com/r/bash/comments/1fb42qr/how_to_progress_bar_on_zstd/

[^14]: https://superuser.com/questions/156673/in-place-extract-tar-archive

[^15]: https://lowendtalk.com/discussion/143309/how-to-check-progress-of-file-transfer-using-tar

[^16]: https://askubuntu.com/questions/392885/how-can-i-view-the-contents-of-tar-gz-file-without-extracting-from-the-command-l

[^17]: https://www.reddit.com/r/bash/comments/ry682d/progress_percentage_with_ongoing_command_not/

[^18]: https://www.tecmint.com/show-progress-linux-commands/

[^19]: https://stackoverflow.com/questions/22141828/how-to-know-when-a-tar-command-finished

[^20]: https://stackoverflow.com/questions/11524602/how-to-extract-tar-archive-from-stdin

[^21]: https://www.linuxquestions.org/questions/linux-software-2/check-tar-progress-4175507868/

