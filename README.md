# ЛАБОРАТОРНА РОБОТА 2 
### Виконав: Угнівенко Ярослав
#### Обраний репозиторій - https://github.com/open-compass/MixtralKit
 #
### Виконання роботи
 Для початку створимо папку і перейдемо туди. Далі зклонуємо репозиторій:

`$ git clone D:/TIRPZ/lab2/MixtralKit`   

Перевіримо наявність гілок:  
`$ git branch -a`     
* main  
  remotes/origin/HEAD -> origin/main  
  remotes/origin/main  
 
Перевіримо, куди "дивиться" наш поточний ремоут origin:  
`$ git remote -v`   
origin  D:/TIRPZ/lab2/MixtralKit (fetch)  
origin  D:/TIRPZ/lab2/MixtralKit (push)  

Додамо ще один ремоут до нашої локальної копії:  
`$ https://github.com/open-compass/MixtralKit.git `  
`$ git remote -v `  
origin  D:/TIRPZ/lab2/MixtralKit (fetch)  
origin  D:/TIRPZ/lab2/MixtralKit (push)  
upstream        https://github.com/open-compass/MixtralKit.git (fetch)  
upstream        https://github.com/open-compass/MixtralKit.git (push)  

Отримаємо список змін з нового ремоута:  
`$ git fetch upstream`  
From https://github.com/open-compass/MixtralKit  
 * [new branch]      hf         -> upstream/hf  
 * [new branch]      main       -> upstream/main  
 * [new branch]      readme     -> upstream/readme  

`$ git branch -a`  
* main  
  remotes/origin/HEAD -> origin/main  
  remotes/origin/main  
  remotes/upstream/hf  
  remotes/upstream/main  
  remotes/upstream/readme  

Тепер створимо нову гілку lab2-branch і створимо в ній пару комітів. Спробуємо зробити push:  
`$ git push`  
fatal: The current branch lab2-branch has no upstream branch.
To push the current branch and set the remote as upstream, use  
   ` git push --set-upstream origin lab2-branch`   
To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

Push не відбувається через те, що ми не вказали в який ремоут його робити, для того щоб Push відбувся потрібно вказати ремоут, використовуються такі команди:  
`$ git push origin lab2-branch`  
`$ git push -u origin lab2-branch`  
параметр -u виконує прив'язку, тобто кожний наступний Push (якщо не змінювати конфігурацію) буде саме у вказаний ремоут. В першому випадку де немає прив'язки потрібно кожний новий Push вказувати ремоут. Приклад:  
`$ git push origin lab2-branch`  
* [new branch]        lab2-branch -> lab2-branch  
`$ git push -u origin lab2-branch`  
Everything up-to-date  
branch 'lab2-branch' set up to track 'origin/lab2-branch'.    
`$ git push`  
Everything up-to-date   

При переході в папку з ЛР1 при перевірці branch побачимо гілку з ЛР2  
`$ git branch`  
  lab2-branch    
  main   

Перевіримо поточний стан гілок:  
`$ git branch -a`  
* lab2-branch  
  main  
  remotes/origin/HEAD -> origin/main  
  remotes/origin/lab2-branch  
  remotes/origin/main  
  remotes/upstream/hf  
  remotes/upstream/main  
  remotes/upstream/readme  

Бачимо, що локальної копії гілки ЛР1 у нас є, але на той випадок, якщо їх не буде - можна використати адресу коміту з віддаленої гілки:  
`$ git merge origin/gilka`  
`$ git log --pretty=oneline --graph`  - для виводу інформації  

Тепер перенесемо коміт до поточної гілки. Для цього використаємо cherry-pick. За допомогою log отримаємо хеш потрібного коміта, потім цей хеш підставимо у cherry-pick, отримаємо:  
 `$ git cherry-pick 345ed71aff74498bd03efdf0f79d456a25137930`  
 [lab2-branch e25115e] added 85  
 Date: Wed Dec 13 17:13:37 2023 +0200  
 1 file changed, 1 insertion(+)  
 create mode 100644 85.txt  
Продемонструємо перенос:
`$ git log --pretty=oneline --graph -n 4 --branches`  
* e25115ee1264c04bc26fb7782f1b6f824409200d (HEAD -> lab2-branch) added 85  
| * 4a3396d4993ffd4012f62c033a8096388e1c34a1 (lab2-branch-2) added 86  
| * 345ed71aff74498bd03efdf0f79d456a25137930 added 85  
| * c213fb0cc2c6d8f82b3d90569308c9ed1e34de2d added 82  
|/  
Ключ --branches означає показати коміти не лише явно назад по історії поточної гілки, а всі
локальні гілки, в тому числі паралельні.  

Для того, щоб визначити останнього спільного предка використовуємо merge-base:  
`$ git merge-base origin/lab2-branch origin/main`  
0874c9b6a3ad8c03454119fe6543202aa15598ab  

Створимо ничку:
`$ git status`  
On branch lab2-branch  
Your branch is ahead of 'origin/lab2-branch' by 3 commits.  
  (use "git push" to publish your local commits)  
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   33.txt  
no changes added to commit (use "git add" and/or "git commit -a")  

Yaroslav@YAR4IK-PC MINGW64 /d/TIRPZ/lab2act/MixtralKit (lab2-branch)  
`$ git stash`  
Saved working directory and index state WIP on lab2-branch: e25115e added 85  
Бачимо, що ничка зберегла останній коміт, виконаємо `git status` ще раз:  
`$ git status`  
On branch lab2-branch  
Your branch is ahead of 'origin/lab2-branch' by 3 commits.  
  (use "git push" to publish your local commits)  
nothing to commit, working tree clean  

Зробимо теж саме для файлів 32.txt i 34.txt:
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   32.txt  
        modified:   34.txt  


Щоб дістати зі стека-нички останній стан можна використати або `git stash  
pop` - він застосує і видалить останній заниканий стан, або ж `git stash apply` - ця команда 
застосує останній збережений стан, але не видалить його з нички. Якщо є більше одного захованого стану - можна передавати повну назву, щоби застосувати до
поточного робочого дерева саме конкретний збережений стан, а не останній: `git stash apply/pop
stash@{0}`

`$ git stash apply stash@{1}`  
On branch lab2-branch    
Your branch is ahead of 'origin/lab2-branch' by 3 commits.    
  (use "git push" to publish your local commits)    
Changes not staged for commit:   
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   33.txt  
no changes added to commit (use "git add" and/or "git commit -a")

Для дослідження gitignore створимо 3 файли:  
Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        1.kvfpm  
        2.kvfpm  
        2023.txt  

В існуючому файлі .gitignore створимо рядок-параметр *.kvfpm:  
`$ git status`  
On branch lab2-branch  
Your branch is ahead of 'origin/lab2-branch' by 4 commits.  
  (use "git push" to publish your local commits)
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   .gitignore  
Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        2023.txt  

Бачимо, що зникли файли із заданим розширенням. Тепер переглянемо приховані файли, для цього використаємо параметр `--ignored`:  
`$ git status --ignored`  
On branch lab2-branch  
Your branch is ahead of 'origin/lab2-branch' by 4 commits.  
  (use "git push" to publish your local commits)  
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   .gitignore  
Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        2023.txt  
Ignored files:  
  (use "git add -f <file>..." to include in what will be committed)  
        1.kvfpm  
        2.kvfpm  

Тепер видалимо ці файли, оскільки вони в статусі untracked, то використаємо `$ git clean -fdx`:  
`$ git clean -fdx`  
Removing 1.kvfpm  
Removing 2.kvfpm  
Removing 2023.txt  

Для перевірки можливості відновлення гілки для початку видалимо її:  
`$ git branch -D lab2-branch`  
Deleted branch lab2-branch (was 1e6ff94).

`$ git push -d origin lab2-branch`  
To D:/TIRPZ/lab2/MixtralKit   - [deleted]         lab2-branch

Тепер подивимося рефлог:  
`$ git reflog`  
0874c9b (HEAD -> main, origin/main, origin/HEAD) HEAD@{0}: checkout: moving from lab2-branch to main  
1e6ff94 HEAD@{1}: commit: changed 32,33,34  
e25115e HEAD@{2}: reset: moving to HEAD  
e25115e HEAD@{3}: reset: moving to HEAD  
e25115e HEAD@{4}: reset: moving to HEAD  
e25115e HEAD@{5}: reset: moving to HEAD  
e25115e HEAD@{6}: checkout: moving from lab2-branch-2 to lab2-branch  
4a3396d (lab2-branch-2) HEAD@{7}: checkout: moving from main to lab2-branch-2  
0874c9b (HEAD -> main, origin/main, origin/HEAD) HEAD@{8}: checkout: moving from lab2-branch to main  
e25115e HEAD@{9}: cherry-pick: added 85  

Створимо нову гілку на основі старої, для цього візьмемо хеш потрібного коміту, який і буде точкою відновлення:  
`$ git branch lab2-resurrected 1e6ff94`  
`$ git checkout lab2-resurrected`  
Switched to branch 'lab2-resurrected'  
`$ git log --pretty=oneline --graph -n 5`  
* 1e6ff941ffb2f752a46cb80b80e052da1fe5461b (HEAD -> lab2-resurrected) changed 32,33,34  
* e25115ee1264c04bc26fb7782f1b6f824409200d added 85  
*   11e955beb0ccfe423b5308497fab65632aabb7ef Merge remote-tracking branch 'upstream/hf' into  lab2-branch  
|\  
| * 8c2204162742c17c56d9c7ad70e5afa02ea34208 (upstream/hf) Update hf  
* | 6f2ea39b724234eb46440de88b1a6a624848f07b added 33, 34  




