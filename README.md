# Despesas Dashboard PRO+

## Como começar
1. Abre o ficheiro `despesas_dashboard_pro_plus.html` diretamente no browser (Chrome, Edge ou Safari). Podes guardá-lo no iCloud Drive/Google Drive e abri-lo localmente.
2. Sempre que abres o ficheiro, a sessão é automaticamente encerrada (o painel fica escondido e as tabelas são limpas) para impedir que dados fiquem visíveis. O primeiro ecrã é sempre o **Login**.
3. Introduz `admin` / `admin` para entrares como administrador e muda de imediato essa password.
4. Depois de entrares, todo o painel fica visível. Sem sessão iniciada não é possível ver tabelas ou exportar dados.

## Utilizadores e acessos
- O admin pode criar/remover utilizadores e definir o seu perfil (Admin, Helder, Goreti ou Conjunto).
- Cada utilizador só vê e edita os dados da sua pessoa ou do Conjunto quando tiver permissão.
- Usa o botão **Sair** para fechar a sessão no dispositivo. Se alguém abrir o ficheiro sem login, não verá dados.

## Registar movimentos e débitos diretos
1. Escolhe a pessoa permitida no seletor (a lista já filtra pelo utilizador ativo).
2. Preenche data, descrição, tipo e categoria e clica em **Adicionar movimento**.
3. Para débitos diretos, indica o dia, descrição, valor e pessoa e grava.

## Metas, dashboard e filtros
- O dashboard mostra totais em tabela apenas para o utilizador autenticado.
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
- Hospedar o HTML em qualquer serviço estático **não** cria sincronização: apenas serve o ficheiro. Cada dispositivo continua a ter a sua cópia local dos dados.
- Para minimizar os passos manuais, usa uma **pasta partilhada** no Google Drive/Dropbox/iCloud:
  - Guarda o backup `.json` nessa pasta; ele fica disponível para todos os dispositivos com acesso à pasta.
  - Depois de atualizares os dados, substitui o ficheiro existente na pasta partilhada (exporta com o mesmo nome e sobrescreve).
  - Nos outros telemóveis, basta abrir a pasta e descarregar esse `.json` atualizado antes de clicar em **Importar backup**.
- Se precisares de sincronização automática entre telemóvel/PC sem exportar/importar, ativa o conector gratuito de Supabase (abaixo). Por padrão a app continua offline e não envia dados para servidores.
- Evita partilhar o ficheiro por links públicos; mantém a pasta privada e só partilhada com as contas de quem pode ver os dados.

## Onde posso hospedar o HTML de forma gratuita e segura?
- Qualquer serviço de alojamento de ficheiros (Google Drive/iCloud/Dropbox) funciona, abrindo o HTML no navegador diretamente a partir da app do serviço.
- Se preferires um link estático fácil de abrir:
  - **GitHub Pages** (gratuito): cria um repositório privado ou público, faz upload do `despesas_dashboard_pro_plus.html` e ativa o Pages. O ficheiro fica disponível num URL HTTPS.
  - **Netlify/Cloudflare Pages/Vercel** (gratuitos para uso pessoal): criam um site estático a partir de um upload ou de um repositório Git. Sobe apenas o HTML para evitar expor backups.
- Em qualquer opção, o login continua obrigatório ao abrir o ficheiro e os dados só ficam no dispositivo até exportares um backup para a tua pasta segura.

## Como criar um backend dedicado gratuito (Supabase) e ligá-lo à app
Se quiseres sincronização automática (sem exportar/importar manual), usa o conector integrado de Supabase (plano gratuito):

1. **Criar conta e projeto**
   - Abre <https://supabase.com>, cria uma conta (plano Free) e um projeto numa região próxima.
   - Vai a *Authentication → Providers* e garante que o *Email/Password* está ativo. Cria utilizadores com o email/password que vais usar nos campos da app.

2. **Criar tabela para guardar o snapshot**
   - Em *SQL*, corre este script (inclui RLS) para guardar um único snapshot por utilizador:
   ```sql
   create table if not exists public.despesas_sync (
     id uuid primary key default gen_random_uuid(),
     user_id uuid not null references auth.users(id),
     payload jsonb not null,
     updated_at timestamptz default now()
   );
   alter table public.despesas_sync enable row level security;
   create policy "owner can select" on public.despesas_sync
     for select using (auth.uid() = user_id);
   create policy "owner can upsert" on public.despesas_sync
     for insert with check (auth.uid() = user_id);
   create policy "owner can update" on public.despesas_sync
     for update using (auth.uid() = user_id);
   ```
   - Depois de executar, confirma no *Table Editor* que a tabela tem as colunas `id`, `user_id`, `payload` e `updated_at` e que a secção de *RLS* mostra as políticas ativas. Se o editor gerar o `id` automaticamente (como na captura), mantém a estrutura acima.

3. **Copiar as chaves públicas**
   - Em *Project Settings → API* copia o **Project URL** e a **anon public key** (nunca uses a service key no HTML).

4. **Configurar no HTML (sem editar código)**
   - Abre o ficheiro e, na secção *Backend dedicado gratuito (Supabase)*, preenche: Project URL, anon key, email e password do utilizador Supabase.
   - Marca *Ativar sync automático* e clica em **Guardar configuração**.
   - Clica em **Pull agora** depois de fazeres login interno (admin/Helder/Goreti) para puxar o snapshot remoto, e o **Push** será feito automaticamente sempre que gravares movimentos/débitos/metas.

5. **Como funciona o sync automático**
   - A app autentica no Supabase com o email/password indicados, faz **pull** ao iniciar sessão e agenda **push** sempre que guardas dados.
   - Os dados continuam filtrados pelo login interno; o Supabase apenas armazena o JSON cifrado pelo próprio serviço com RLS por utilizador.

6. **Preciso sempre de internet?**
   - **Sim, para o Supabase**: pulls/pushes só funcionam com ligação ativa. Se estiveres offline, continuas a trabalhar localmente e o push fica em fila; quando a internet voltar, o conector envia a versão mais recente.
   - **Não, para o resto da app**: tudo o que é local (login interno, registos, metas, exportar backup) funciona offline. Se quiseres parar de usar o backend, desmarca *Ativar sync automático* e guarda.

6. **Boas práticas de segurança**
   - Usa sempre HTTPS e mantém o HTML privado; não publiques a chave *service* em lado nenhum.
   - Se quiseres desativar o backend, desmarca a opção *Ativar sync automático* e guarda novamente.

Assim tens sincronização automática gratuita entre telemóvel e PC sem mexer em código adicional.
