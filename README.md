# Vagrantfiles

汎用的に使えるようにしたVagrantfiles
（ただの勉強用です）

# Usage

## Windows

※コマンドプロンプト等は管理者権限で起動すること

事前にしておかなければならない設定
```
fsutil behavior set SymlinkEvaluation L2L:1 R2R:1 L2R:1 R2L:1
vagrant plugin install vagrant-docker-compose
```
参考：
- https://qiita.com/noobar/items/eedf1235c3fbd9744b76
- https://qiita.com/tanakachang/items/11dab5309b64b4f64119

```
git clone https://github.com/konabe/Vagrantfiles/
cd Vagrantfiles
# Vagrantfilesをいじる
mkdir data
# data/に開発に必要なデータを入れる（gitリポジトリなど）
vagrant up
```
