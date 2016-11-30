#01. create repo (repo A)
mkdir repoA
cd repoA/
git init

#02. repo A: create 1st commit with some files (file A, file B)
vim A.txt
vim B.txt
git add A.txt B.txt
git commit -m "First commit of A and B"

#03. repo A: create branch branch1 from master
git branch branch1

#04. clone the current repo (repo B)
git checkout branch1
cd ..
mkdir repoB
cd repoB
git clone ../RepoA .

#05. repo B, branch1: create 2nd commit containing new file (file C)
vim C.txt
git add C.txt
git commit -m 'Second commit, repo B, branch1'

#06. repo B: push changes to repo A
cd ../repoA
git checkout master #Почему-то без этого он ни за что не хотел push'ить
cd ../repoB
git push origin branch1

#07. repo A, branch1: modify line#1 in file C and commit
cd ../repoA
git checkout branch1
nano C.txt
git add C.txt
git commit -m "Third commit: first line of C.txt changed in repoA"

#08. repo B, branch1: modify line#1 in file C and commit
cd ../repoB
nano C.txt
git add C.txt
git commit -m "Fourth commit: first line of C.txt changed in repoB"

#09. repo B, branch1: fetch changes from repo A
git fetch origin

#10. repo B, branch1: merge in repoA's branch1 (resolve conflict)
git merge origin/branch1 #тут случается конфликт и в файл добавляется запись о том, что он имеет различия в разных репозиториях
git mergetool --tool=emerge #вылез очень непонятный редактор
git commit -m "merging"

#11. repo A: add repoB as new remote
cd ../repoA
git remote add repoB ../repoB

#12. repo A, branch1: merge in repoB's branch1
git merge origin/branch1 #not something we can merge
git merge branch1 #Already up-to-date.
# что ж, попробуем ещё такой вариант
git pull repoB branch1

----------------------

git remote add origin https://github.com/Udimus/repoA.git
git push -u origin master
git push -u origin branch1
cd ../repoB
git remote add origin-2 https://github.com/Udimus/repoB.git
git push -u origin-2 branch1

