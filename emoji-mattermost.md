# emoji-mattermost

## mattermostでbotを作成する
- adminアカウントでログインし、左上のメニューアイコンから統合機能をクリック
- 「Botアカウントを追加する」を押して、「システム管理者」の役割を持ったBotを作成
- トークンが表示される（閉じたら2度と表示されない）ので、必ずメモ

## botのユーザIDを取得する
```
$ TOKEN=<Botのトークン>
$ CREATE_ID=$(curl -s -X GET -H "Authorization: Bearer ${TOKEN}" https://<Mattermost URL>/api/v4/users/me | jq -r .id)
$ echo $CREATE_ID
```
- <Botのトークン>を先ほど取得したトークンに置き換える
- <Mattermost URL>をホストしているMattermostのURL（mattermost.sohosai.com など）に置き換える

## 絵文字を一括で登録する
```
$ cd <絵文字ファイルが入ったディレクトリ>
$ TOKEN=<Botのトークン>
$ CREATE_ID=<BotのユーザID>
$ for file in $(ls)
do
    name=$(echo $file | cut -d . -f 1)

    curl -X POST -H "Authorization: Bearer ${TOKEN}" \
             -H "Content-Type: multipart/form-data" \
             -F "emoji={\"name\":\"${name}\",\"creator_id\":\"${CREATE_ID}\"}" -F "image=@./${file}" \
             https://<Mattermost URL>/api/v4/emoji
done
```