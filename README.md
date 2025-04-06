# How To Git On Linux

## Como configurar uma chave GPG para autenticação do GitHub e assinatura de _commits_ pelo terminal do Linux

> Apesar do tutorial funcionar para o Git do Windows, outros passos podem ser necessários.

## Sumário

1. [Instalar o _Git Credential Manager_ (GCM)](#1º-passo);
2. [Configurar o GCM](#2º-passo);
3. [Gerar uma chave GPG](#3º-passo);
    1. [(Opcional) Importação e exportação de chave privada GPG](#passo-opcional).
4. [Configurar o Git para usar a _Credential Store_ GPG](#4º-passo);
5. [Exportar a chave GPG pública](#5º-passo);
6. [Contar ao Git sobre sua chave GPG](#6º-passo);
7. [Configurar a chave GPG para a assinatura dos _commits_](#7º-passo);
8. [Contar ao Git seu nome e e-mail para a assinatura dos commits](#8º-passo).

---

### 1º Passo:

__Instalar o _Git Credential Manager_ (GCM):__
> Fonte: https://github.com/git-ecosystem/git-credential-manager/blob/release/docs/install.md

- Baixe o pacote: [gcm-linux-amd64.x.x.x.deb](https://github.com/git-ecosystem/git-credential-manager/releases/tag/v2.5.1);
- E instale: `sudo dpkg -i ./gcm-linux_amd64.x.x.x.deb`.

---

### 2º Passo:

__Configurar o GCM:__
> Fonte: https://github.com/git-ecosystem/git-credential-manager/blob/release/docs/install.md

- `git-credential-manager configure`.

---

### 3º Passo:

__Gerar uma chave GPG:__
> Fonte: https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key

- Execute: `gpg --full-generate-key`
- Escolha as opções padrão (aperte `enter` três vezes);
- Assine seu nome e e-mail;
> O e-mail deve ser o mesmo do GitHub.

- Insira "GitHub" como comentário;
- Digite "o" (de "ok") para confirmar a geração da chave GPG;
- Insira a senha que será usada para autenticar a sessão do Git (em cada novo terminal).

---

### Passo opcional:

__Importação e exportação de chave privada GPG:__

O [3º passo](#3º-passo) pode ser substituído pela importação de uma __chave existente__.

Isto é, após __exportar__ a chave privada:

- `gpg --export-secret-keys --armor <gpg-key-id> > gpg-private-key.asc`.

A informação da chave privata estará disponível no arquivo `gpg-private-key.asc`.
> Qualquer nome é válido, contanto que a extensão seja `.asc`.

Dessa forma, em outra máquina, faça a __importação__ da chave privada a partir do arquivo:

- `gpg --import ./gpg-private-key.asc`.

E confira o nível de confiança máximo à chave GPG:

- `gpg --edit-key <gpg-key-id>`;
- `trust`;
- `5`;
> (5 é maior nível de confiança)
- `quit`.

---

### 4º Passo:

__Configurar o Git para usar a _Credential Store_ GPG:__
> Fonte: https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/credstores.md

- `git config --global credential.credentialStore gpg`

---

### 5º Passo:

__Exportar a chave GPG pública:__
> Fonte: https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key

- Liste as chaves GPG em formato longo: `gpg --list-secret-keys --keyid-format=long`;
- Identifique a chave com comentário "GitHub" e copie o identificador da chave GPG:

![Passo 5 1](https://github.com/user-attachments/assets/0072c89b-0c27-42f6-a721-ce367ee4a50f)
> Figura 1: Identificador da chave GPG.

- Inicie o _pass_ para autenticar o _login_ do GitHub (referente ao e-mail inserido no 3º passo): `pass init <gpg-key-id>`;
> Instale o _pass_ com `sudo apt install pass`.

- Exporte a chave GPG pública: `gpg --armor --export <gpg-key-id>`.

![Passo 5 2](https://github.com/user-attachments/assets/dabaee90-3636-4727-8be5-bce0998c3f22)
> Figura 2: Chave GPG pública.

---

### 6º Passo:

__Contar ao Git sobre sua chave GPG:__
> Fonte: https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key

- Copie a chave GPG pública desde "-----BEGIN PGP PUBLIC KEY BLOCK-----" até "-----END PGP PUBLIC KEY BLOCK-----";
- No [GitHub](https://github.com/), acesse _Settings_ ➡️ _SSH and GPG keys_ ➡️ _New GPG key_;
- Escolha qualquer título e cole a chave GPG pública em _Key_:

![Passo 6](https://github.com/user-attachments/assets/5d22cfc5-8147-4920-9eab-477796b7626d)
> Figura 3: Chave GPG pública no _website_ GitHub.

---

### 7º Passo:

__Configurar a chave GPG para a assinatura dos _commits_:__

- Copie o identificador da chave GPG (obtido no 3º passo): `git config --global user.signingkey <gpg-key-id>`;

- Configure o Git para assinar os _commits_ por padrão: `git config --global commit.gpgsign true`;
> Este passo pode ser omitido. Contudo, é altamente recomendado.

- Configura a variável de ambiente GPG_TTY: `[ -f ~/.bashrc ] && echo -e '\nexport GPG_TTY=$(tty)' >> ~/.bashrc`.
> Garantindo que operações GPG que exigem interação (como assinatura) funcionem no terminal atual.

---

### 8º Passo:

__Contar ao Git seu nome e e-mail para a assinatura dos commits:__

- `git config --global user.name "<nome>"`;
> "nome" é o nome de usuário do GitHub.

- `git config --global user.email "<e-mail>"`.
> O e-mail deve ser o mesmo do GitHub.

Para visualização da configuração, acesse o arquivo oculto `.gitconfig` na pasta pessoal de usuário.

---

Feito com amor por Yumiowari 🪶


