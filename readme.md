# 필수사항 powershell로 설치인데 [무조건 관리자권한으로 실행]
## 깃허브 레포짓에서 settings -actions -runners로 진행 => 내용들 수행
# 문제가 있다면 Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process로 권한 허용

# 실행 과정에서 폴더 위치에 따라 실행하는데 문제가 생길 수 있으므로 조심할것


name: play

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build:
    # runs-on: ubuntu-latest - 이건 github에서 제공하는 가상환경, 윈도우에서 설정하기 위해 self-hosted로 변경
    runs-on: self-hosted
    steps:
      # - uses: actions/checkout@v
      # - name: Check PowerShell - 확인용
      #   run: $PSVersionTable
      - name: Checkout repository
        uses: actions/checkout@v4
        # with:
        #   path: .
      # - name: Check Node.js version
      #   run: node -v

      - name: npm install
        working-directory: ./Front
        run: npm i
      
      - name: npm run build
        working-directory: ./Front
        run: npm run build

     - name: nginx start
        run: docker run -d -p 80:80 -v ./frontend/dist:/usr/share/nginx/html --name web nginx:1.28