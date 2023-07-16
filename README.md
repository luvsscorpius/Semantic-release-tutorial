# Semantic Release Tutorial 

1. First you need to create a folder and open on VsCode.
2. Then on tour terminal you write **npm init** and you’re gojng to fill your informations.
3. Then we need to create a file index.js on the root of our project.
4. Then we need to install semantic release by typing **npm install --save-dev semantic-release.**
5. And then we need to install some semantic plugins **npm install @semantic-release/changelog @semantic-release/git -D.**
6. Now we need to create the configuration file name it **.releaserc.json.**

{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    [
      "@semantic-release/changelog",
      {
        "changelogFile": "CHANGELOG.md"
      }
    ],
    "@semantic-release/npm",
    "@semantic-release/github",
    [
      "@semantic-release/git",
      {
        "assets": ["package.json", "CHANGELOG.md"],
        "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
      }
    ]
  ]
}

7. Now we will configure the github actions, first you need to create .github > workflows > release.yml and then you paste this code:

name: Release
on:
  push:
    branches:
      - main

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "lts/*"

      - name: Install dependencies
        run: npm ci

      - name: Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release

8. Now you need to create a file .gitignore to ignore the folder node_modules
9. Now you can create a new repo on github and upload, if you want you can add commitizen here.
10. If your workflow fail, you need to go to settings > actions > generals > workflow permissions > Read and write.
11. Now you need to go npm web site, log > access token > generate new token > name > GITHUB_TOKEN > publish.
12. Copy your token and on your repo on github go to settings > secrets and variables > actions > new repository secret > name > NPM_TOKEN > and paste your secret