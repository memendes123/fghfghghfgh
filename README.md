# Despesas Dashboard PRO+

## Como começar
1. Abre o ficheiro `despesas_dashboard_pro_plus.html` diretamente no browser (Chrome, Edge ou Safari). Podes guardá-lo no iCloud Drive/Google Drive e abri-lo localmente.
2. Sempre que abres o ficheiro, a sessão é automaticamente encerrada para impedir que dados fiquem visíveis. O primeiro ecrã é sempre o **Login**.
3. Introduz `admin` / `admin` para entrares como administrador e muda de imediato essa password.
4. Depois de entrares, todo o painel fica visível. Sem sessão iniciada não é possível ver tabelas, gráficos ou exportar dados.

## Utilizadores e acessos
- O admin pode criar/remover utilizadores e definir o seu perfil (Admin, Helder, Goreti ou Conjunto).
- Cada utilizador só vê e edita os dados da sua pessoa ou do Conjunto quando tiver permissão.
- Usa o botão **Sair** para fechar a sessão no dispositivo. Se alguém abrir o ficheiro sem login, não verá dados.

## Registar movimentos e débitos diretos
1. Escolhe a pessoa permitida no seletor (a lista já filtra pelo utilizador ativo).
2. Preenche data, descrição, tipo e categoria e clica em **Adicionar movimento**.
3. Para débitos diretos, indica o dia, descrição, valor e pessoa e grava.

## Metas, dashboard e filtros
- O dashboard mostra totais e gráficos apenas para o utilizador autenticado.
- Define metas mensais nas caixas próprias e grava com **Guardar metas**.
- Usa o filtro de mês para navegar entre períodos.

## Backups, exportação e sincronização
- Exporta um ficheiro `.json` em **Backup e recuperação** para salvaguardar tudo (inclui utilizadores e configurações).
- Importa o backup num outro dispositivo para alinhar os dados e volta a exportar sempre que fizeres alterações.
- Usa **Exportar Excel/CSV** ou **Copiar para Google Sheets** para partilhar dados sem expor acesso total.
- O botão **Forçar sincronização** só sincroniza separadores/PCs a correr o mesmo ficheiro em simultâneo.

## Guardar no Google Drive ou iCloud
- Carrega o ficheiro HTML para a tua Drive (Google Drive) ou iCloud Drive. 
- No telemóvel, abre o ficheiro a partir da app Drive/iCloud e escolhe abrir no navegador (Safari/Chrome). O login continua a ser obrigatório sempre que abrires.
- Mantém uma cópia de segurança do `.json` no mesmo serviço para poderes restaurar rapidamente se mudares de dispositivo.

## Usar no telemóvel
- **iPhone:** abre o HTML no Safari → Partilhar → *Adicionar ao ecrã principal* para criar um atalho em ecrã inteiro.
- **Android:** abre no Chrome → menu ⋮ → *Adicionar à tela inicial*.
- Cada telemóvel guarda os dados localmente; usa o backup `.json` para manter dois dispositivos alinhados.

## Usar em dois telemóveis com os mesmos dados
1. Num telemóvel, depois de atualizares dados, exporta o backup `.json`.
2. Guarda esse `.json` no Google Drive/iCloud.
3. No segundo telemóvel, descarrega o `.json` e faz **Importar backup**. O login é sempre requerido após a importação.
4. Sempre que fizeres alterações em qualquer telemóvel, volta a exportar o backup e repete o passo 3 no outro para manter a informação sincronizada.

## Posso sincronizar sem exportar/importar sempre?
- Não existe sincronização automática em tempo real porque a app não tem servidor próprio nem base de dados online. Tudo fica guardado localmente no navegador de cada dispositivo para manter a privacidade.
- Para minimizar os passos manuais, usa uma **pasta partilhada** no Google Drive/Dropbox/iCloud:
  - Guarda o backup `.json` nessa pasta; ele fica disponível para todos os dispositivos com acesso à pasta.
  - Depois de atualizares os dados, substitui o ficheiro existente na pasta partilhada (exporta com o mesmo nome e sobrescreve).
  - Nos outros telemóveis, basta abrir a pasta e descarregar esse `.json` atualizado antes de clicar em **Importar backup**.
- Evita partilhar o ficheiro por links públicos; mantém a pasta privada e só partilhada com as contas de quem pode ver os dados.

## Onde posso hospedar o HTML de forma gratuita e segura?
- Qualquer serviço de alojamento de ficheiros (Google Drive/iCloud/Dropbox) funciona, abrindo o HTML no navegador diretamente a partir da app do serviço.
- Se preferires um link estático fácil de abrir:
  - **GitHub Pages** (gratuito): cria um repositório privado ou público, faz upload do `despesas_dashboard_pro_plus.html` e ativa o Pages. O ficheiro fica disponível num URL HTTPS.
  - **Netlify/Cloudflare Pages/Vercel** (gratuitos para uso pessoal): criam um site estático a partir de um upload ou de um repositório Git. Sobe apenas o HTML para evitar expor backups.
- Em qualquer opção, o login continua obrigatório ao abrir o ficheiro e os dados só ficam no dispositivo até exportares um backup para a tua pasta segura.
