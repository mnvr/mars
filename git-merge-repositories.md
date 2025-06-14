```sh
git clone https://github.com/mnvr/mnvr.github.io mnvr.in
cd mnvr.in
git remote add old https://github.com/mnvr/mnvr.in
git fetch old
git merge --allow-unrelated-histories old/main
git remote remove old
```

