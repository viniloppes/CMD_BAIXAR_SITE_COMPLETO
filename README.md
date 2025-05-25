# Como Baixar um Site Inteiro no Windows (com WSL e Wget)

Este guia ensinará como baixar um site completo para o seu computador Windows, utilizando o **Windows Subsystem for Linux (WSL)** e a ferramenta de linha de comando **Wget**.

---

## 1. Instalar o Chocolatey (Gerenciador de Pacotes para Windows)

O Chocolatey simplifica a instalação de softwares no Windows. Se você já tem o Chocolatey instalado, pule para a próxima etapa.

1.  Abra o **Windows PowerShell como administrador**. Para fazer isso, digite "PowerShell" na barra de pesquisa do Windows, clique com o botão direito em "Windows PowerShell" e selecione "Executar como administrador".
2.  Execute o seguinte comando para instalar o Chocolatey. Este comando permite a execução de scripts e configura o protocolo de segurança necessário:

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```
    Se você vir um aviso de que o Chocolatey já está instalado, não se preocupe. Prossiga para a próxima etapa.

---

## 2. Instalar o WSL (Windows Subsystem for Linux)

O WSL permite que você execute um ambiente Linux diretamente no Windows, o que é essencial para usar o Wget de forma eficaz.

1.  No **Windows PowerShell (como administrador)**, execute o comando de instalação do WSL:

    ```powershell
    wsl --install
    ```
2.  O Windows fará o download e a instalação do WSL e de uma distribuição Linux padrão (geralmente o Ubuntu). Este processo pode levar alguns minutos.
3.  Após a instalação, você será solicitado a criar um nome de usuário e senha para o seu ambiente Linux. Anote-os, pois você precisará deles mais tarde.

---

## 3. Acessar o Ambiente Linux (Ubuntu)

Agora que o WSL está instalado, você pode acessar seu ambiente Linux para usar o Wget.

1.  No **Windows PowerShell**, digite o seguinte comando para iniciar o Ubuntu:

    ```powershell
    wsl.exe -d Ubuntu
    ```
    Você verá o prompt do terminal Linux, que se parece com `seu_usuario@seu_computador:/mnt/c/WINDOWS/system32$`.

---

## 4. Baixar o Site com Wget

Dentro do ambiente Linux (Ubuntu), você usará o comando `wget` para baixar o site.

1.  Certifique-se de que o **Wget** está instalado no seu ambiente Ubuntu. Geralmente, ele já vem pré-instalado. Se não estiver, você pode instalá-lo com:
    ```bash
    sudo apt update
    sudo apt install wget
    ```
    Você precisará da sua senha do Ubuntu para isso.

2.  Escolha um **diretório no Windows** onde você deseja salvar o site. No exemplo, foi usado `D:\Users\lopes\Documentos\BKP_GERAL\site-ednalopes`. No WSL, os drives do Windows são montados sob `/mnt/`. Então, `D:\Users\lopes\Documentos\BKP_GERAL\site-ednalopes` se torna `/mnt/d/Users/lopes/Documentos/BKP_GERAL/site-ednalopes`.

3.  Execute o seguinte comando no terminal Ubuntu, substituindo o URL do site e o caminho de destino pelo seu:

    ```bash
    wget --mirror --convert-links --adjust-extension --page-requisites --no-parent -P "/mnt/d/Users/lopes/Documentos/BKP_GERAL/site-ednalopes" https://abdomesemdiastase.com.br/
    ```

    * `--mirror`: Ativa o modo de espelhamento, que é ideal para baixar um site completo.
    * `--convert-links`: Converte os links no HTML para que funcionem localmente.
    * `--adjust-extension`: Adiciona extensões de arquivo (como `.html`) a arquivos sem extensão.
    * `--page-requisites`: Baixa todos os arquivos necessários para exibir uma página HTML (imagens, CSS, JavaScript, etc.).
    * `--no-parent`: Não sobe para diretórios pais ao seguir links, garantindo que você baixe apenas o conteúdo do site especificado.
    * `-P "/mnt/d/Users/lopes/Documentos/BKP_GERAL/site-ednalopes"`: Especifica o diretório de destino onde o site será salvo. **Lembre-se de usar o formato de caminho Linux (`/mnt/drive_letter/`) e aspas duplas se o caminho contiver espaços.**
    * `https://abdomesemdiastase.com.br/`: O URL do site que você deseja baixar.

    O Wget começará a baixar os arquivos do site para o diretório especificado. Você verá o progresso no terminal.

---

## Observações Importantes:

* **Erros "utime Operation not permitted"**: Você pode ver avisos como "utime(...): Operation not permitted" durante o download. Isso geralmente acontece ao salvar arquivos de um sistema Linux (WSL) em um sistema de arquivos Windows e não afeta o download ou a funcionalidade do site baixado.
* **Robots.txt e 404 Not Found**: É normal ver erros como "404 Not Found" para `robots.txt` ou outros arquivos que podem não existir no servidor. Isso não impede o download do restante do site.
* **Tamanho do Site**: Sites grandes com muitos arquivos podem demorar um pouco para baixar.
* **Visualizando o Site Offline**: Após o download, você pode navegar até o diretório onde o site foi salvo (no seu Windows Explorer) e abrir o arquivo `index.html` (ou o arquivo principal do site) no seu navegador para visualizá-lo offline.

Com esses passos, você conseguirá baixar e espelhar sites completos no seu sistema Windows usando as poderosas ferramentas do WSL e Wget.
