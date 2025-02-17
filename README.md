Documentação Técnica: Release Candidate Workflow

Descrição

Este workflow tem como objetivo criar uma nova Release Candidate (RC) a partir de uma branch Feature Candidate (FC) ou de um Hotfix. O processo envolve:

•	Obter a última release marcada como latest.

•	Calcular a nova versão com base na origem:

•	Feature Candidate (FC): incrementa a versão minor.

•	Hotfix: incrementa a versão patch.

•	Criar a nova branch RC, se necessário.

•	Criar uma release e tag no GitHub.

•	Abrir um Pull Request (PR) para merge da Feature Candidate na RC.

Este workflow é implementado como uma GitHub Action Composite, permitindo reutilização em diferentes pipelines de CI/CD.

# Parâmetros de Entrada

CICD_GITHUB_TOKEN - Obrigatório - Token de autenticação para chamadas à API do GitHub.
ORIGIN - Obrigatório - Define se a branch de origem é uma Feature Candidate (fc) ou um Hotfix (hotfix).

# Etapas do Workflow

1. Checkout do Código
•	Baixa o código do repositório para a máquina de execução.
•	Utiliza a action actions/checkout@v4.

2. Obter a Última Release Tag
•	Busca a última tag de release marcada como latest via API do GitHub.
•	Se não houver nenhuma release marcada como latest, o workflow falha.

3. Verificar a Branch Atual
•	Obtém o nome da branch atual e armazena como CURRENT_BRANCH.
•	Define a variável TAG com a última versão da release.

4. Instalar Ferramenta Semver
•	Baixa e instala a ferramenta semver, que é usada para manipulação de versões de software.

5. Determinar Nova Versão
•	Se a origem (ORIGIN) for fc, incrementa a versão minor (exemplo: 1.2.3 → 1.3.0).
•	Se for um hotfix, incrementa a versão patch (exemplo: 1.2.3 → 1.2.4).

6. Verificar Existência da Branch RC
•	Consulta a API do GitHub para verificar se a branch rc/{TAG} já existe no repositório.
•	Caso exista, define a variável exists=true.

7. Criar PR ou Nova RC
•	Se a RC já existe: Abre um Pull Request (PR) da Feature Candidate para a branch RC existente.
•	Se a RC não existe:
•	Atualiza a branch main e cria a nova branch rc/{TAG}.
•	Publica a nova release e tag no GitHub.
•	Abre um Pull Request para merge da Feature Candidate na nova branch RC.

# Considerações

•	Este workflow depende de a branch seguir um padrão específico para extrair corretamente a versão.

•	Se a branch RC já existir, apenas um PR será criado.

•	Se a branch RC não existir, ela será criada e a release será publicada automaticamente.

•	O GitHub Actions precisa ter permissões para criar e fazer push de branches no repositório.