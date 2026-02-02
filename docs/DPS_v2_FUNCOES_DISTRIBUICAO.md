# DPS v2 — Catálogo de Funções e Distribuição por Módulos (Complementar)

> Documento complementar para recriação do sistema. Consolida as **APIs públicas** (funções globais e métodos) do DPS e sugere uma distribuição moderna entre **Core** e **Add-ons**.

> Fonte: *DPS Functions Reference* (v2.6.0, atualizado em dezembro/2024). Gerado em 02/02/2026.


---

## Índice de módulos

- [Base Plugin (Core)](#base-plugin-core)
- [Client Portal Add-on](#client-portal-add-on)
- [Communications Add-on](#communications-add-on)
- [Finance Add-on](#finance-add-on)
- [Loyalty Add-on](#loyalty-add-on)
- [Push Add-on](#push-add-on)
- [AI Add-on](#ai-add-on)
- [Agenda Add-on](#agenda-add-on)
- [Stats Add-on](#stats-add-on)
- [Services Add-on](#services-add-on)
- [Backup Add-on](#backup-add-on)
- [Booking Add-on](#booking-add-on)
- [Groomers Add-on](#groomers-add-on)
- [Payment Add-on](#payment-add-on)
- [Registration Add-on](#registration-add-on)
- [Stock Add-on](#stock-add-on)
- [Subscription Add-on](#subscription-add-on)

---

## Base Plugin (Core)

### Funções globais

- `dps_get_template()` — Localiza e carrega um template, permitindo override pelo tema.
- `dps_get_template_path()` — Retorna o caminho do template que seria carregado, sem incluí-lo.
- `dps_is_template_overridden()` — Verifica se um template está sendo sobrescrito pelo tema.

### APIs (classes e métodos públicos)

#### DPS_Addon_Manager
*Gerenciador central de add-ons. Fornece listagem, categorização e verificação de instalação.*
- `DPS_Addon_Manager::get_instance()` — Gerenciador de Add-ons do DPS.
- `DPS_Addon_Manager::get_all_addons()` — Construtor privado para singleton.
- `DPS_Addon_Manager::get_categories()` — Retorna categorias de add-ons com labels traduzidos.
- `DPS_Addon_Manager::get_addons_by_category()` — Retorna add-ons agrupados por categoria.
- `DPS_Addon_Manager::is_installed()` — Verifica se um add-on está instalado (arquivo existe).
- `DPS_Addon_Manager::is_active()` — Verifica se um add-on está ativo.
- `DPS_Addon_Manager::get_addon_file()` — Retorna o caminho completo do arquivo principal do add-on.
- `DPS_Addon_Manager::get_dependents()` — Retorna add-ons que dependem de um determinado add-on.

#### DPS_URL_Builder
*Helper para construção consistente de URLs de edição, exclusão e visualização.*
- `DPS_URL_Builder::build_edit_url()` — Helper class para construção de URLs do painel.
- `DPS_URL_Builder::build_delete_url()` — Constrói URL para excluir um registro com nonce de segurança.
- `DPS_URL_Builder::build_view_url()` — Constrói URL para visualizar detalhes de um registro.
- `DPS_URL_Builder::build_tab_url()` — Constrói URL para uma aba específica.
- `DPS_URL_Builder::build_schedule_url()` — Constrói URL para agendar atendimento para um cliente específico.
- `DPS_URL_Builder::remove_action_params()` — Remove parâmetros de ação da URL.
- `DPS_URL_Builder::safe_get_permalink()` — Safe wrapper for get_permalink() that always returns a string.
- `DPS_URL_Builder::get_clean_current_url()` — Obtém URL base da página atual sem parâmetros de ação.

#### DPS_Cache_Control
*Controle de cache: desabilita cache para páginas com shortcodes DPS, evitando conteúdo desatualizado.*
- `DPS_Cache_Control::init()` — Classe responsável pelo controle de cache das páginas do DPS.
- `DPS_Cache_Control::maybe_disable_cache_by_url_params()` — Desabilita cache baseado em parâmetros de URL específicos do DPS.
- `DPS_Cache_Control::maybe_disable_page_cache()` — Verifica se a página atual contém shortcodes DPS e desabilita cache. Este método é executado no hook 'template_redirect', antes que qualquer output seja enviado ao navegador.
- `DPS_Cache_Control::disable_cache()` — Verifica se o conteúdo da página atual contém shortcodes do DPS.
- `DPS_Cache_Control::send_nocache_headers()` — Envia os headers HTTP de no-cache. Este método é chamado tanto pelo hook 'send_headers' quanto diretamente quando necessário.
- `DPS_Cache_Control::disable_admin_cache()` — Desabilita cache para páginas administrativas do DPS. Garante que todas as páginas admin do DPS não sejam cacheadas, independente de shortcodes.
- `DPS_Cache_Control::force_no_cache()` — Método público para forçar desabilitação de cache.
- `DPS_Cache_Control::register_shortcode()` — Adiciona um shortcode à lista de shortcodes DPS. Permite que add-ons registrem seus próprios shortcodes para desabilitação automática de cache.

#### DPS_CPT_Helper
*Helper para registrar Custom Post Types com opções padronizadas.*
- `DPS_CPT_Helper::register()` — Executa o registro do CPT com argumentos opcionais adicionais.

#### DPS_GitHub_Updater
*Sistema de atualização automática via GitHub Releases.*
- `DPS_GitHub_Updater::get_instance()` — DPS GitHub Updater Classe responsável por verificar e gerenciar atualizações dos plugins DPS diretamente do repositório GitHub.
- `DPS_GitHub_Updater::check_for_updates()` — Construtor privado para singleton.
- `DPS_GitHub_Updater::plugin_info()` — Fornece informações detalhadas do plugin para o popup.
- `DPS_GitHub_Updater::after_install()` — Obtém dados da release mais recente do GitHub.
- `DPS_GitHub_Updater::maybe_force_check()` — Verifica se deve forçar checagem de atualizações. Requer nonce válido para proteção CSRF.
- `DPS_GitHub_Updater::update_notice()` — Exibe aviso sobre atualizações disponíveis.
- `DPS_GitHub_Updater::force_check()` — Método público para forçar verificação de atualizações.
- `DPS_GitHub_Updater::get_managed_plugins()` — Retorna a lista de plugins gerenciados.

#### DPS_Logger
*Sistema centralizado de logs do DPS.*
- `DPS_Logger::log()` — Registra log genérico.
- `DPS_Logger::debug()` — Registra log de debug.
- `DPS_Logger::info()` — Registra log de informação.
- `DPS_Logger::warning()` — Registra log de aviso.
- `DPS_Logger::error()` — Registra log de erro.

#### DPS_Request_Validator
*Validação de requisições, nonces e capabilities.*
- `DPS_Request_Validator::verify_ajax_nonce()` — Verifica nonce para requisições AJAX.
- `DPS_Request_Validator::verify_ajax_admin()` — Verifica nonce e capability para AJAX admin.
- `DPS_Request_Validator::verify_admin_form()` — Verifica nonce de formulário admin (POST).
- `DPS_Request_Validator::get_post_int()` — Obtém e sanitiza valor inteiro do POST.
- `DPS_Request_Validator::get_post_string()` — Obtém e sanitiza string do POST.
- `DPS_Request_Validator::get_post_textarea()` — Obtém e sanitiza textarea do POST.
- `DPS_Request_Validator::get_post_checkbox()` — Obtém valor de checkbox do POST.
- `DPS_Request_Validator::send_json_success()` — Envia resposta JSON de sucesso padronizada.
- `DPS_Request_Validator::send_json_error()` — Envia resposta JSON de erro padronizada.

#### DPS_Client_Helper
*Centraliza acesso a dados de clientes, seguindo o princípio DRY. Suporta tanto CPT dps_client quanto usuários WordPress.*
- `DPS_Client_Helper::get_phone()` — Obtém o número de telefone do cliente.
- `DPS_Client_Helper::get_email()` — Obtém o endereço de email do cliente.
- `DPS_Client_Helper::get_whatsapp()` — Obtém o número WhatsApp do cliente.
- `DPS_Client_Helper::get_name()` — Obtém o nome do cliente.
- `DPS_Client_Helper::get_display_name()` — Obtém o nome do cliente formatado para UI.
- `DPS_Client_Helper::get_address()` — Obtém o endereço completo formatado.
- `DPS_Client_Helper::get_all_data()` — Obtém todos os metadados do cliente de uma só vez.
- `DPS_Client_Helper::has_valid_phone()` — Verifica se o cliente tem um número de telefone válido.
- `DPS_Client_Helper::has_valid_email()` — Verifica se o cliente tem um email válido.
- `DPS_Client_Helper::get_pets()` — Obtém os pets associados ao cliente.
- `DPS_Client_Helper::get_pets_count()` — Obtém a contagem de pets do cliente.
- `DPS_Client_Helper::get_primary_pet()` — Obtém o primeiro pet do cliente.
- `DPS_Client_Helper::format_contact_info()` — Formata informações de contato como HTML.
- `DPS_Client_Helper::get_for_display()` — Obtém dados do cliente formatados e prontos para UI.
- `DPS_Client_Helper::search_by_phone()` — Busca cliente por número de telefone.
- `DPS_Client_Helper::search_by_email()` — Busca cliente por email.

#### DPS_Money_Helper
*Utilitários para conversão e formatação de valores monetários.*
- `DPS_Money_Helper::parse_brazilian_format()` — Converte string em formato brasileiro para centavos.
- `DPS_Money_Helper::format_to_brazilian()` — Formata valor em centavos para string no formato brasileiro.
- `DPS_Money_Helper::format_currency()` — Formata valor em centavos com símbolo de moeda.
- `DPS_Money_Helper::format_currency_from_decimal()` — Formata valor decimal (reais) com símbolo.
- `DPS_Money_Helper::decimal_to_cents()` — Converte valor decimal para centavos.
- `DPS_Money_Helper::cents_to_decimal()` — Converte centavos para valor decimal.
- `DPS_Money_Helper::format_decimal_to_brazilian()` — Formata valor decimal para formato brasileiro.
- `DPS_Money_Helper::is_valid_money_string()` — Valida se string representa valor monetário válido.
- `DPS_Money_Helper::sanitize_post_price_field()` — Sanitiza e converte campo de preço do POST para float.

#### DPS_Query_Helper
*Utilitários para construção de consultas WP_Query padronizadas e eficientes.*
- `DPS_Query_Helper::build_base_query_args()` — Constrói argumentos base para consulta de posts.
- `DPS_Query_Helper::get_all_posts_by_type()` — Obtém todos os posts de um tipo específico.
- `DPS_Query_Helper::get_paginated_posts()` — Obtém posts paginados.
- `DPS_Query_Helper::count_posts_by_type()` — Obtém contagem de posts.

#### DPS_Phone_Helper
*Formatação e validação de números de telefone brasileiros.*
- `DPS_Phone_Helper::format_for_whatsapp()` — Formata número para WhatsApp (formato internacional).
- `DPS_Phone_Helper::format_for_display()` — Formata número para exibição no formato brasileiro.
- `DPS_Phone_Helper::is_valid_brazilian_phone()` — Valida se número é um telefone brasileiro válido.

#### DPS_Message_Helper
*Helper para mensagens/avisos padronizados (ex.: sucesso/erro) em fluxos do DPS (admin e/ou frontend).*
- `DPS_Message_Helper::add_success()` — Adiciona uma mensagem de sucesso (notice) padronizada.
- `DPS_Message_Helper::add_error()` — Adiciona uma mensagem de erro (notice) padronizada.

#### DPS_IP_Helper
*Helper para obtenção segura do IP do visitante/cliente (com validação e fallback).*
- `DPS_IP_Helper::get_ip()` — Retorna o IP do cliente/visitante de forma segura (com validações e fallback).

#### DPS_Admin_Tabs_Helper
*Helper para organização e renderização de abas/tabs no admin do DPS.*
- *(Sem métodos públicos listados no documento de referência.)*

## Client Portal Add-on

### Funções globais

- `dps_get_portal_page_url()` — Obtém a URL da página do Portal do Cliente.
- `dps_get_portal_page_id()` — Obtém o ID da página do Portal do Cliente.
- `dps_get_page_by_title_compat()` — Busca uma página pelo título de forma compatível com WordPress 6.2+.
- `dps_get_tosa_consent_page_url()` — Obtém a URL da página de Consentimento de Tosa com Máquina.
- `dps_portal_assert_client_owns_resource()` — Valida se um recurso pertence ao cliente autenticado.

### APIs (classes e métodos públicos)

#### DPS_Portal_Session_Manager
*Gerenciamento de sessões autenticadas de clientes.*
- `DPS_Portal_Session_Manager::get_instance()` — Gerenciador de sessões do Portal do Cliente Esta classe gerencia a autenticação e sessão dos clientes no portal, independente do sistema de usuários do WordPress.
- `DPS_Portal_Session_Manager::authenticate_client()` — Construtor privado para singleton
- `DPS_Portal_Session_Manager::get_authenticated_client_id()` — Retorna o ID do cliente autenticado
- `DPS_Portal_Session_Manager::is_authenticated()` — Verifica se há um cliente autenticado
- `DPS_Portal_Session_Manager::validate_session()` — Valida a sessão atual Remove sessões expiradas ou inválidas
- `DPS_Portal_Session_Manager::logout()` — Faz logout do cliente

#### DPS_Portal_Token_Manager
*Geração e validação de tokens de acesso único.*
- `DPS_Portal_Token_Manager::get_instance()` — Gerenciador de tokens de acesso ao Portal do Cliente Esta classe gerencia a criação, validação, revogação e limpeza de tokens de autenticação para o Portal do Cliente.
- `DPS_Portal_Token_Manager::maybe_create_table()` — Construtor privado para singleton
- `DPS_Portal_Token_Manager::generate_token()` — Cria a tabela de tokens
- `DPS_Portal_Token_Manager::validate_token()` — Valida um token e retorna os dados se válido Implementa rate limiting para prevenir brute force: - 5 tentativas por hora por IP - Cache negativo de tokens inválidos (5 min) - Logg…
- `DPS_Portal_Token_Manager::mark_as_used()` — Incrementa o contador de rate limiting
- `DPS_Portal_Token_Manager::revoke_tokens()` — Revoga todos os tokens ativos de um cliente

#### DPS_Client_Repository
*Repositório: consulta otimizada de dados de clientes.*
- `DPS_Client_Repository::get_instance()` — Repositório para operações de dados relacionadas a clientes.
- `DPS_Client_Repository::get_client_by_id()` — Construtor privado (singleton).
- `DPS_Client_Repository::get_client_by_email()` — Busca um cliente por email.
- `DPS_Client_Repository::get_client_by_phone()` — Busca um cliente por telefone.
- `DPS_Client_Repository::get_clients()` — Busca todos os clientes com paginação.

#### DPS_Pet_Repository
*Repositório: consulta de pets vinculados a clientes.*
- `DPS_Pet_Repository::get_instance()` — Repositório para operações de dados relacionadas a pets.
- `DPS_Pet_Repository::get_pet()` — Construtor privado (singleton).
- `DPS_Pet_Repository::get_pets_by_client()` — Busca todos os pets de um cliente.
- `DPS_Pet_Repository::pet_belongs_to_client()` — Verifica se um pet pertence a um cliente.

#### DPS_Appointment_Repository
*Repositório de agendamentos do Portal do Cliente.*
- *(Sem métodos públicos listados no documento de referência.)*

#### DPS_Finance_Repository
*Repositório financeiro do Portal do Cliente (transações, pendências, etc.).*
- *(Sem métodos públicos listados no documento de referência.)*

## Communications Add-on

### APIs (classes e métodos públicos)

#### DPS_Communications_API
*API principal para envio de comunicações. Interface única para WhatsApp, Email e SMS.*
- `DPS_Communications_API::get_instance()` — API centralizada de comunicações Esta classe centraliza toda a lógica de envio de comunicações (WhatsApp, e-mail, SMS) no sistema DPS.
- `DPS_Communications_API::get_last_error()` — Construtor privado (singleton)
- `DPS_Communications_API::send_whatsapp()` — Registra log de forma segura, verificando disponibilidade do DPS_Logger.
- `DPS_Communications_API::send_email()` — Envia e-mail Método central para envio de e-mails no sistema.
- `DPS_Communications_API::send_appointment_reminder()` — Envia lembrete de agendamento Método específico para envio de lembretes de agendamentos. Busca dados do agendamento e usa template configurado.
- `DPS_Communications_API::send_payment_notification()` — Envia notificação de pagamento

#### DPS_Communications_History
*Gerenciamento de histórico: rastreamento e consulta de mensagens enviadas.*
- `DPS_Communications_History::get_instance()` — Gerenciador de histórico de comunicações Esta classe gerencia a tabela de histórico de comunicações, registrando todas as mensagens enviadas (WhatsApp, e-mail, SMS).
- `DPS_Communications_History::get_table_name()` — Construtor
- `DPS_Communications_History::table_exists()` — Verifica se a tabela existe
- `DPS_Communications_History::maybe_create_table()` — Cria ou atualiza a tabela de histórico
- `DPS_Communications_History::log_communication()` — Registra uma nova comunicação no histórico
- `DPS_Communications_History::update_status()` — Atualiza o status de uma comunicação

#### DPS_Communications_Retry
*Sistema de retry automático para mensagens que falharam.*
- `DPS_Communications_Retry::get_instance()` — Gerenciador de retry com exponential backoff Esta classe implementa lógica de retry com exponential backoff para falhas de envio de comunicações.
- `DPS_Communications_Retry::schedule_retry()` — Construtor
- `DPS_Communications_Retry::process_retry()` — Processa o retry de uma comunicação
- `DPS_Communications_Retry::cleanup_expired_retries()` — Calcula o delay do backoff exponencial com jitter
- `DPS_Communications_Retry::get_stats()` — Obtém estatísticas de retries

#### DPS_Communications_Webhook
*Webhooks para receber confirmações de status de mensagens.*
- `DPS_Communications_Webhook::get_instance()` — Gerenciador de webhooks de status de entrega Esta classe gerencia webhooks recebidos de gateways de comunicação para atualizar o status de entrega das mensagens.
- `DPS_Communications_Webhook::maybe_generate_secret()` — Construtor
- `DPS_Communications_Webhook::get_secret()` — Obtém o secret do webhook
- `DPS_Communications_Webhook::get_webhook_url()` — Obtém a URL do webhook
- `DPS_Communications_Webhook::register_routes()` — Registra rotas REST
- `DPS_Communications_Webhook::verify_webhook()` — Verifica autenticidade do webhook

#### DPS_WhatsApp_Helper
*Geração de links e mensagens padronizadas para WhatsApp.*
- `DPS_WhatsApp_Helper::get_link_to_team()` — Gera link WhatsApp para cliente enviar mensagem à equipe.
- `DPS_WhatsApp_Helper::get_link_to_client()` — Gera link WhatsApp para equipe enviar mensagem ao cliente.
- `DPS_WhatsApp_Helper::get_portal_access_request_message()` — Gera mensagem padrão para cliente solicitar acesso ao portal.
- `DPS_WhatsApp_Helper::get_portal_link_message()` — Gera mensagem padrão para envio de link do portal ao cliente.
- `DPS_WhatsApp_Helper::get_appointment_confirmation_message()` — Gera mensagem de confirmação de agendamento.
- `DPS_WhatsApp_Helper::get_payment_request_message()` — Gera mensagem de cobrança.

## Finance Add-on

### APIs (classes e métodos públicos)

#### DPS_Finance_API
*API principal: criação/atualização de cobranças, marcação de pagamentos, consultas.*
- `DPS_Finance_API::create_or_update_charge()` — API Financeira Centralizada do DPS Fornece interface pública para operações financeiras, centralizando toda a lógica de criação, atualização e consulta de cobranças/transações.
- `DPS_Finance_API::mark_as_paid()` — Disparado após atualizar uma cobrança existente.
- `DPS_Finance_API::mark_as_pending()` — Disparado quando uma cobrança é marcada como paga. Hook mantido para compatibilidade com Loyalty e outros add-ons.
- `DPS_Finance_API::mark_as_cancelled()` — Marcar cobrança como cancelada.
- `DPS_Finance_API::get_charge()` — Buscar dados de uma cobrança.
- `DPS_Finance_API::get_charges_by_appointment()` — Buscar todas as cobranças de um agendamento.
- `DPS_Finance_API::delete_charges_by_appointment()` — Remover todas as cobranças de um agendamento. Usado quando agendamento é excluído. Remove também parcelas vinculadas.

#### DPS_Finance_Audit
*Auditoria: rastreamento de alterações em transações financeiras.*
- `DPS_Finance_Audit::init()` — Gerencia auditoria de alterações financeiras.
- `DPS_Finance_Audit::log_event()` — Registra evento de auditoria.
- `DPS_Finance_Audit::get_logs()` — Obtém IP do cliente de forma segura.
- `DPS_Finance_Audit::count_logs()` — Conta total de logs de auditoria.
- `DPS_Finance_Audit::register_audit_page()` — Registra página de auditoria no menu admin.
- `DPS_Finance_Audit::render_audit_page()` — Renderiza página de auditoria.

#### DPS_Finance_Reminders
*Sistema de lembretes automáticos para pagamentos pendentes.*
- `DPS_Finance_Reminders::init()` — Gerencia lembretes automáticos de pagamento.
- `DPS_Finance_Reminders::clear_scheduled_hook()` — Limpa evento cron agendado.
- `DPS_Finance_Reminders::process_reminders()` — Processa lembretes de pagamento (executado diariamente via cron).
- `DPS_Finance_Reminders::is_enabled()` — Envia lembretes ANTES do vencimento.
- `DPS_Finance_Reminders::render_settings_section()` — Renderiza seção de configurações de lembretes.
- `DPS_Finance_Reminders::save_settings()` — Salva configurações de lembretes.

#### DPS_Finance_Revenue_Query
*Consultas otimizadas de receita e métricas financeiras.*
- `DPS_Finance_Revenue_Query::sum_by_period()` — Helper para consultar faturamento a partir de metas históricas.

## Loyalty Add-on

### APIs (classes e métodos públicos)

#### DPS_Loyalty_API
*API pública do sistema de fidelidade.*
- `DPS_Loyalty_API::add_points()` — Adiciona pontos ao cliente.
- `DPS_Loyalty_API::get_points()` — Obtém saldo de pontos do cliente.
- `DPS_Loyalty_API::redeem_points()` — Resgata pontos do cliente.
- `DPS_Loyalty_API::add_credit()` — Adiciona crédito ao cliente (em centavos).
- `DPS_Loyalty_API::get_credit()` — Obtém saldo de crédito do cliente (em centavos).
- `DPS_Loyalty_API::use_credit()` — Usa crédito do cliente.
- `DPS_Loyalty_API::get_referral_code()` — Obtém código de indicação do cliente.
- `DPS_Loyalty_API::get_referral_url()` — Obtém URL de indicação do cliente.
- `DPS_Loyalty_API::get_loyalty_tier()` — Obtém nível de fidelidade do cliente.
- `DPS_Loyalty_API::get_top_clients()` — Obtém ranking dos melhores clientes.

#### DPS_Loyalty_Achievements
*API de conquistas/achievements do programa de fidelidade (regras e premiações).*
- *(Sem métodos públicos listados no documento de referência.)*

## Push Add-on

### APIs (classes e métodos públicos)

#### DPS_Push_API
*API de push notifications usando Web Push Protocol (VAPID).*
- `DPS_Push_API::generate_vapid_keys()` — API de Push Notifications para o DPS. Implementa Web Push API usando biblioteca PHP nativa.
- `DPS_Push_API::send_to_user()` — Envia notificação para um usuário específico.
- `DPS_Push_API::send_to_all_admins()` — Envia notificação para todos os administradores.
- `DPS_Push_API::dps_ai_log_debug()` — Logger condicional para o AI Add-on.
- `DPS_Push_API::dps_ai_log_info()` — Registra uma mensagem informativa. Útil para eventos normais do sistema que valem documentação. Não é registrado em produção (a menos que debug_logging esteja habilitado).
- `DPS_Push_API::dps_ai_log_warning()` — Registra uma mensagem de aviso. Indica situações anormais que não são necessariamente erros. Não é registrado em produção (a menos que debug_logging esteja habilitado).
- `DPS_Push_API::dps_ai_log_error()` — Registra uma mensagem de erro. Indica falhas críticas que requerem atenção. Sempre é registrado, mesmo em produção.

#### DPS_Email_Reports
*Geração e envio de relatórios por e-mail (ex.: para admin ou rotinas).*
- *(Sem métodos públicos listados no documento de referência.)*

## AI Add-on

### Funções globais

- `dps_ai_log()` — Logger genérico do AI Add-on com escolha de nível e contexto (não detalhado no documento).
- `dps_ai_log_debug()` — Logger condicional para o AI Add-on.
- `dps_ai_log_info()` — Registra uma mensagem informativa. Útil para eventos normais do sistema que valem documentação. Não é registrado em produção (a menos que debug_logging esteja habilitado).
- `dps_ai_log_warning()` — Registra uma mensagem de aviso. Indica situações anormais que não são necessariamente erros. Não é registrado em produção (a menos que debug_logging esteja habilitado).
- `dps_ai_log_error()` — Registra uma mensagem de erro. Indica falhas críticas que requerem atenção. Sempre é registrado, mesmo em produção.
- `dps_ai_log_conversation()` — Registra mensagens de uma conversa (role + conteúdo) vinculadas a um ID de conversa (não detalhado no documento).

### APIs (classes e métodos públicos)

#### DPS_AI_Assistant
*Assistente principal: processamento de mensagens e geração de respostas.*
- `DPS_AI_Assistant::answer_portal_question()` — Assistente de IA do DPS. Este arquivo contém a classe responsável por todas as regras de negócio da IA, incluindo o system prompt restritivo e a montagem de contexto.
- `DPS_AI_Assistant::get_base_system_prompt()` — Verifica se a pergunta contém palavras-chave do contexto permitido.
- `DPS_AI_Assistant::get_base_system_prompt_with_language()` — Retorna o prompt base do sistema com instrução de idioma. Adiciona instrução explícita para que a IA responda no idioma configurado.
- `DPS_AI_Assistant::invalidate_context_cache()` — Obtém contexto do cliente com cache via Transients. Cacheia o contexto por 5 minutos para evitar reconstrução repetitiva a cada pergunta do mesmo cliente.

#### DPS_AI_Knowledge_Base
*Base de conhecimento: busca semântica e contextual.*
- `DPS_AI_Knowledge_Base::get_instance()` — Base de Conhecimento do AI Add-on. Gerencia artigos e FAQs que são incluídos no contexto da IA para respostas mais precisas e personalizadas.
- `DPS_AI_Knowledge_Base::register_post_type()` — Construtor privado.
- `DPS_AI_Knowledge_Base::register_taxonomy()` — Registra a taxonomia de categorias.
- `DPS_AI_Knowledge_Base::add_meta_boxes()` — Cria termos padrão da taxonomia. Chamado apenas uma vez durante a primeira inicialização.

#### DPS_AI_Client
*Cliente HTTP para consumo de provedores de IA (ex.: OpenAI) com autenticação, retries e timeouts.*
- *(Sem métodos públicos listados no documento de referência.)*

## Agenda Add-on

### APIs (classes e métodos públicos)

#### DPS_Agenda_Capacity_Helper
*Gerenciamento de capacidade: slots disponíveis por período.*
- `DPS_Agenda_Capacity_Helper::get_default_capacity_config()` — Helper para gerenciamento de capacidade e lotação da AGENDA.
- `DPS_Agenda_Capacity_Helper::get_capacity_config()` — Obtém a configuração de capacidade atual.
- `DPS_Agenda_Capacity_Helper::save_capacity_config()` — Salva a configuração de capacidade.
- `DPS_Agenda_Capacity_Helper::get_capacity_for_period()` — Retorna a capacidade para um slot específico.
- `DPS_Agenda_Capacity_Helper::get_period_from_time()` — Determina o período (morning/afternoon) baseado em um horário.
- `DPS_Agenda_Capacity_Helper::get_capacity_heatmap_data()` — Retorna dados de heatmap de capacidade para um intervalo de datas.

#### DPS_Agenda_GPS_Helper
*Funcionalidades GPS: cálculo de rotas e distâncias.*
- `DPS_Agenda_GPS_Helper::get_shop_address()` — Helper para geração de rotas GPS na AGENDA. Centraliza a lógica de construção de URLs do Google Maps para rotas, SEMPRE do endereço do Banho e Tosa até o endereço do cliente.
- `DPS_Agenda_GPS_Helper::get_client_address()` — Retorna o endereço do cliente de um agendamento.
- `DPS_Agenda_GPS_Helper::get_route_url()` — Monta a URL de rota do Google Maps. IMPORTANTE: SEMPRE monta a rota do Banho e Tosa até o cliente. Não implementa o trajeto inverso nesta fase.
- `DPS_Agenda_GPS_Helper::render_route_button()` — Renderiza botão "Abrir rota" se houver dados suficientes.
- `DPS_Agenda_GPS_Helper::render_map_link()` — Renderiza link de mapa simples (apenas destino, sem rota). Mantido para compatibilidade com o código existente.
- `DPS_Agenda_GPS_Helper::is_shop_address_configured()` — Verifica se a configuração de endereço da loja está definida.

#### DPS_Agenda_Payment_Helper
*Helper para processar pagamentos de agendamentos.*
- `DPS_Agenda_Payment_Helper::get_payment_status()` — Helper para consolidar status de pagamento na AGENDA. Centraliza a lógica de obtenção de status de pagamento, evitando duplicação de código entre diferentes componentes da agenda.
- `DPS_Agenda_Payment_Helper::get_payment_badge_config()` — Retorna a configuração de badge para um status de pagamento.
- `DPS_Agenda_Payment_Helper::get_payment_details()` — Retorna detalhes de pagamento para tooltip/popover.
- `DPS_Agenda_Payment_Helper::render_payment_badge()` — Renderiza badge de status de pagamento.
- `DPS_Agenda_Payment_Helper::render_payment_tooltip()` — Renderiza tooltip com detalhes de pagamento.
- `DPS_Agenda_Payment_Helper::render_resend_button()` — Renderiza botão "Reenviar link de pagamento" se aplicável.

## Stats Add-on

### APIs (classes e métodos públicos)

#### DPS_Stats_API
*API de estatísticas: métricas de agendamentos, receita e performance.*
- `DPS_Stats_API::bump_cache_version()` — API pública do Stats Add-on Centraliza toda a lógica de estatísticas e métricas para reutilização por outros add-ons e facilitar manutenção.
- `DPS_Stats_API::get_appointments_count()` — Obtém contagem de atendimentos no período.
- `DPS_Stats_API::get_revenue_total()` — Obtém total de receitas pagas no período.
- `DPS_Stats_API::get_expenses_total()` — Obtém total de despesas pagas no período.
- `DPS_Stats_API::get_financial_totals()` — Obtém totais financeiros do período (receita e despesas).
- `DPS_Stats_API::get_inactive_pets()` — Obtém pets inativos (sem atendimento há X dias).
- `DPS_Stats_API::get_top_services()` — Obtém serviços mais solicitados no período.
- `DPS_Stats_API::get_period_comparison()` — Calcula comparativo entre período atual e período anterior.

## Services Add-on

### APIs (classes e métodos públicos)

#### DPS_Services_API
*API de serviços: CRUD e consulta de serviços disponíveis.*
- `DPS_Services_API::get_service()` — API pública do Services Add-on Centraliza toda a lógica de serviços, cálculo de preços e informações detalhadas para reutilização por outros add-ons (Agenda, Finance, Portal, etc.)
- `DPS_Services_API::calculate_price()` — Calcula o preço de um serviço com base no porte do pet.
- `DPS_Services_API::calculate_appointment_total()` — Calcula o total de um agendamento com base nos serviços e pets selecionados.
- `DPS_Services_API::get_services_details()` — Obtém detalhes de serviços de um agendamento. Estrutura retornada: [ 'services' => [ ['name' => string, 'price' => float], ... ], 'total' => float, ]
- `DPS_Services_API::calculate_package_price()` — Normaliza o porte do pet para formato padrão.
- `DPS_Services_API::get_price_history()` — Obtém o histórico de alterações de preço de um serviço.

## Backup Add-on

### APIs (classes e métodos públicos)

#### DPS_Backup_Addon
*Classe principal de gerenciamento; registra menus, renderiza interface administrativa, processa formulários e requisições AJAX.*
- `DPS_Backup_Addon::get_instance()` — Retorna a instância singleton do add-on.
- `DPS_Backup_Addon::register_admin_menu()` — Registra o submenu "Backup" no admin do WordPress sob o menu principal "desi.pet by PRObst".
- `DPS_Backup_Addon::enqueue_admin_assets()` — Enfileira CSS e JavaScript para a página de backup no admin.
- `DPS_Backup_Addon::render_admin_page()` — Renderiza a página principal de backup e restauração no admin, incluindo configurações, botões de ação e histórico.
- `DPS_Backup_Addon::handle_save_settings()` — Processa o formulário de configurações de agendamento de backup.
- `DPS_Backup_Addon::handle_export()` — Processa a requisição de exportação manual de backup (download JSON).
- `DPS_Backup_Addon::handle_import()` — Processa o upload e restauração de arquivo de backup.
- `DPS_Backup_Addon::ajax_compare_backup()` — Endpoint AJAX para comparar backup com dados atuais.
- `DPS_Backup_Addon::ajax_delete_backup()` — Endpoint AJAX para deletar backup do histórico.
- `DPS_Backup_Addon::ajax_download_backup()` — Endpoint AJAX para baixar backup do histórico.
- `DPS_Backup_Addon::ajax_restore_from_history()` — Endpoint AJAX para restaurar backup do histórico.

#### DPS_Backup_Exporter
*Exportador de dados em formatos completo, seletivo ou diferencial.*
- `DPS_Backup_Exporter::build_complete_backup()` — Cria backup completo de todos os componentes disponíveis.
- `DPS_Backup_Exporter::build_selective_backup()` — Cria backup seletivo com componentes especificados.
- `DPS_Backup_Exporter::build_differential_backup()` — Cria backup diferencial desde uma data específica (apenas registros modificados).
- `DPS_Backup_Exporter::export_transactions()` — Exporta transações financeiras com validação de relacionamentos.
- `DPS_Backup_Exporter::get_component_counts()` — Retorna contagem de registros para todos os componentes disponíveis.

#### DPS_Backup_History
*Gerencia registros de histórico de backups e armazenamento de arquivos.*
- `DPS_Backup_History::get_history()` — Recupera histórico de backups, ordenado do mais recente para o mais antigo.
- `DPS_Backup_History::add_entry()` — Adiciona nova entrada ao histórico, aplicando retenção automática.
- `DPS_Backup_History::remove_entry()` — Remove backup do histórico e deleta o arquivo.
- `DPS_Backup_History::save_backup_file()` — Salva conteúdo JSON do backup no disco com segurança.
- `DPS_Backup_History::format_size()` — Formata bytes para formato legível (KB, MB, GB).

#### DPS_Backup_Scheduler
*Gerencia agendamento automático de backups via WordPress cron.*
- `DPS_Backup_Scheduler::init()` — Inicializa hooks e filtros do agendador.
- `DPS_Backup_Scheduler::schedule()` — Agenda backup automático baseado nas configurações.
- `DPS_Backup_Scheduler::is_scheduled()` — Verifica se backup está agendado.
- `DPS_Backup_Scheduler::get_next_run()` — Retorna timestamp da próxima execução agendada.

#### DPS_Backup_Comparator
*Compara dados de backup com estado atual do sistema.*
- `DPS_Backup_Comparator::compare()` — Compara backup com dados atuais, retorna comparação detalhada.
- `DPS_Backup_Comparator::format_summary()` — Formata comparação como tabela HTML com avisos.

#### DPS_Backup_Settings
*Gerencia configurações de operações de backup.*
- `DPS_Backup_Settings::get_all()` — Recupera todas as configurações com defaults mesclados.
- `DPS_Backup_Settings::get()` — Obtém valor de uma configuração específica.
- `DPS_Backup_Settings::set()` — Define valor de uma configuração.
- `DPS_Backup_Settings::get_available_components()` — Retorna componentes disponíveis para backup.
- `DPS_Backup_Settings::dps_booking_check_base_plugin()` — Verifica se o plugin base está ativo; exibe aviso de erro se ausente.
- `DPS_Backup_Settings::dps_booking_load_textdomain()` — Carrega arquivos de tradução para o add-on de booking.
- `DPS_Backup_Settings::dps_booking_init_addon()` — Inicializa a instância singleton do Booking Add-on.

## Booking Add-on

### Funções globais

- `dps_booking_check_base_plugin()` — Verifica se o plugin base está ativo; exibe aviso de erro se ausente.
- `dps_booking_load_textdomain()` — Carrega arquivos de tradução para o add-on de booking.
- `dps_booking_init_addon()` — Inicializa a instância singleton do Booking Add-on.

### APIs (classes e métodos públicos)

#### DPS_Booking_Addon
*Classe principal fornecendo página dedicada de agendamento com mesma funcionalidade do Painel de Gestão DPS.*
- `DPS_Booking_Addon::get_instance()` — Retorna instância singleton do add-on.
- `DPS_Booking_Addon::activate()` — Cria página de agendamento na ativação do plugin.
- `DPS_Booking_Addon::enqueue_assets()` — Enfileira CSS/JS para página de agendamento; carrega apenas na página de booking ou onde o shortcode existe.
- `DPS_Booking_Addon::render_booking_form()` — Renderiza formulário completo de agendamento com verificações de permissão.
- `DPS_Booking_Addon::capture_saved_appointment()` — Captura dados de agendamento salvo e armazena em transient para exibição de confirmação.

## Groomers Add-on

### APIs (classes e métodos públicos)

#### DPS_Groomers_Addon
*Classe principal do add-on gerenciando perfis de staff, portal via shortcodes, avaliações e comissões.*
- `DPS_Groomers_Addon::get_instance()` — Recupera instância singleton do add-on.
- `DPS_Groomers_Addon::get_portal_page_url()` — Retorna URL da página do portal de tosadores.
- `DPS_Groomers_Addon::render_groomer_portal_shortcode()` — Renderiza shortcode [dps_groomer_portal] com dashboard, agenda e abas de avaliações.
- `DPS_Groomers_Addon::render_groomer_dashboard_shortcode()` — Renderiza shortcode [dps_groomer_dashboard] com stats e gráficos de desempenho.
- `DPS_Groomers_Addon::render_groomer_agenda_shortcode()` — Renderiza shortcode [dps_groomer_agenda] com calendário de agendamentos do tosador.
- `DPS_Groomers_Addon::generate_staff_commission()` — Gera automaticamente comissões de staff quando pagamento é confirmado; divide proporcionalmente entre staff vinculado.
- `DPS_Groomers_Addon::get_groomer_rating()` — Retorna avaliação média do tosador e contagem total de avaliações.
- `DPS_Groomers_Addon::get_staff_types()` — Retorna tipos de staff disponíveis com traduções.
- `DPS_Groomers_Addon::activate()` — Adiciona role dps_groomer na ativação do plugin.

#### DPS_Groomer_Session_Manager
*Gerencia autenticação e sessões do portal de tosadores via magic links sem login tradicional.*
- `DPS_Groomer_Session_Manager::get_instance()` — Recupera a instância singleton do gerenciador de sessões.
- `DPS_Groomer_Session_Manager::authenticate_groomer()` — Autentica um tosador, retorna true em caso de sucesso; valida role do usuário e regenera session ID.
- `DPS_Groomer_Session_Manager::get_authenticated_groomer_id()` — Retorna ID do tosador autenticado ou 0 se não autenticado.
- `DPS_Groomer_Session_Manager::is_groomer_authenticated()` — Verifica se algum tosador está atualmente autenticado.
- `DPS_Groomer_Session_Manager::validate_session()` — Valida expiração da sessão atual (tempo de vida de 24h).
- `DPS_Groomer_Session_Manager::logout()` — Limpa dados de sessão do tosador.
- `DPS_Groomer_Session_Manager::get_logout_url()` — Gera URL de logout com nonce e parâmetro de redirecionamento opcional.
- `DPS_Groomer_Session_Manager::get_authenticated_groomer()` — Retorna objeto WP_User do tosador autenticado ou false.

#### DPS_Groomer_Token_Manager
*Gerencia geração, validação, revogação e limpeza de tokens de magic link para acesso ao portal.*
- `DPS_Groomer_Token_Manager::get_instance()` — Recupera a instância singleton do gerenciador de tokens.
- `DPS_Groomer_Token_Manager::generate_token()` — Gera novo token de acesso; retorna token em texto plano ou false em caso de erro.
- `DPS_Groomer_Token_Manager::validate_token()` — Valida token e retorna dados se válido; verifica expiração, uso e status de revogação.
- `DPS_Groomer_Token_Manager::revoke_tokens()` — Revoga todos os tokens ativos de um tosador; retorna contagem de revogados ou false em erro.
- `DPS_Groomer_Token_Manager::get_groomer_stats()` — Retorna estatísticas de tokens: total_generated, total_used, active_tokens, last_used_at.
- `DPS_Groomer_Token_Manager::get_active_tokens()` — Lista todos os tokens ativos de um tosador com ID, tipo, datas e IP.

## Payment Add-on

### APIs (classes e métodos públicos)

#### DPS_Payment_Addon
*Gerenciador principal de integração MercadoPago: geração de links, webhooks e injeção de informações de pagamento em mensagens.*
- `DPS_Payment_Addon::get_instance()` — Recupera instância singleton do add-on de pagamentos.
- `DPS_Payment_Addon::enqueue_admin_assets()` — Enfileira CSS e JavaScript na página de configurações de pagamento.
- `DPS_Payment_Addon::register_settings()` — Registra configurações WordPress para access token, chave PIX e webhook secret com callbacks de sanitização.
- `DPS_Payment_Addon::sanitize_access_token()` — Sanitiza access token do MercadoPago - remove espaços e caracteres inválidos.
- `DPS_Payment_Addon::sanitize_webhook_secret()` — Sanitiza webhook secret - remove caracteres de controle mas permite especiais para senhas fortes.
- `DPS_Payment_Addon::add_settings_page()` — Adiciona página de configurações no submenu "desi.pet by PRObst".
- `DPS_Payment_Addon::render_settings_page()` — Renderiza página completa de configurações de pagamento com indicador de status.
- `DPS_Payment_Addon::maybe_generate_payment_link()` — Gera link de pagamento para agendamentos finalizados e armazena como post meta.
- `DPS_Payment_Addon::inject_payment_link_in_message()` — Filtro que injeta link de pagamento e informações PIX em mensagens WhatsApp.
- `DPS_Payment_Addon::maybe_handle_mp_notification()` — addon->maybe_handle_mp_notification()

#### DPS_MercadoPago_Config
*Gerencia credenciais seguras do MercadoPago com sistema de fallback prioritário (constantes → opções do banco).*
- `DPS_MercadoPago_Config::get_access_token()` — Recupera access token do MercadoPago.
- `DPS_MercadoPago_Config::get_public_key()` — Recupera public key do MercadoPago.
- `DPS_MercadoPago_Config::get_webhook_secret()` — Recupera webhook secret para validação.
- `DPS_MercadoPago_Config::is_access_token_from_constant()` — Verifica se access token é definido via constante DPS_MERCADOPAGO_ACCESS_TOKEN.
- `DPS_MercadoPago_Config::get_masked_credential()` — Retorna credencial mascarada para exibição segura na UI.
- `DPS_MercadoPago_Config::dps_registration_check_base_plugin()` — Verifica se o plugin base DPS está ativo; exibe aviso administrativo se ausente.
- `DPS_MercadoPago_Config::dps_registration_load_textdomain()` — Carrega domínio de tradução do plugin para localização.

## Registration Add-on

### Funções globais

- `dps_registration_check_base_plugin()` — Verifica se o plugin base DPS está ativo; exibe aviso administrativo se ausente.
- `dps_registration_load_textdomain()` — Carrega domínio de tradução do plugin para localização.

### APIs (classes e métodos públicos)

#### DPS_Registration_Addon
*Classe principal gerenciando formulário de registro de clientes/pets, confirmação por email, API endpoints e configurações.*
- `DPS_Registration_Addon::get_instance()` — Retorna instância singleton do add-on.
- `DPS_Registration_Addon::deactivate()` — Limpeza na desativação do plugin.
- `DPS_Registration_Addon::activate()` — Cria página de registro na ativação do plugin.
- `DPS_Registration_Addon::register_settings()` — Registra configurações WordPress para configuração do plugin.
- `DPS_Registration_Addon::render_settings_page()` — Renderiza página de configurações no admin.
- `DPS_Registration_Addon::render_pending_clients_page()` — Renderiza lista de confirmações de clientes pendentes.
- `DPS_Registration_Addon::register_rest_routes()` — Registra endpoint REST API para registro.
- `DPS_Registration_Addon::rest_register_permission_check()` — addon->rest_register_permission_check($request)
- `DPS_Registration_Addon::handle_rest_register()` — Processa registro de cliente via REST API.
- `DPS_Registration_Addon::maybe_handle_registration()` — Processa submissão de formulário do frontend.
- `DPS_Registration_Addon::maybe_handle_email_confirmation()` — Processa confirmação de email via token na URL.
- `DPS_Registration_Addon::render_registration_form()` — Renderiza shortcode de formulário de registro multi-etapa.
- `DPS_Registration_Addon::send_confirmation_reminders()` — Envia emails de lembrete para clientes não confirmados após 24h.
- `DPS_Registration_Addon::get_pet_fieldset_html()` — Gera HTML para um único fieldset de pet.
- `DPS_Registration_Addon::dps_stock_check_base_plugin()` — Verifica se o plugin base DPS está ativo antes de carregar o add-on.
- `DPS_Registration_Addon::dps_stock_load_textdomain()` — Carrega domínio de texto para traduções do Stock add-on.
- `DPS_Registration_Addon::dps_stock_init_addon()` — Inicializa o Stock add-on após disparo do hook init.

## Stock Add-on

### Funções globais

- `dps_stock_check_base_plugin()` — Verifica se o plugin base DPS está ativo antes de carregar o add-on.
- `dps_stock_load_textdomain()` — Carrega domínio de texto para traduções do Stock add-on.
- `dps_stock_init_addon()` — Inicializa o Stock add-on após disparo do hook init.

### APIs (classes e métodos públicos)

#### DPS_Stock_Addon
*Classe principal gerenciando sistema de inventário, registro de CPT, integração de UI e dedução de estoque.*
- `DPS_Stock_Addon::register_stock_cpt()` — Registra custom post type para itens de estoque.
- `DPS_Stock_Addon::register_meta_boxes()` — Adiciona meta box 'dps_stock_details' ao CPT de estoque para edição.
- `DPS_Stock_Addon::render_stock_metabox()` — Renderiza UI da metabox com campos de unidade, quantidade e quantidade mínima.
- `DPS_Stock_Addon::save_stock_meta()` — Salva unidade, quantidade e valores mínimos em post meta com validação.
- `DPS_Stock_Addon::can_access_stock()` — Verifica se usuário atual tem capability de gestão de estoque ou é admin.
- `DPS_Stock_Addon::add_stock_tab()` — Adiciona aba "Estoque" à navegação do dashboard principal.
- `DPS_Stock_Addon::add_stock_section()` — Renderiza seção de gestão de estoque no dashboard principal.
- `DPS_Stock_Addon::render_stock_page()` — Retorna HTML completo para página de inventário de estoque.
- `DPS_Stock_Addon::maybe_handle_appointment_completion()` — Deduz automaticamente estoque quando agendamento é finalizado.
- `DPS_Stock_Addon::activate()` — Executa na ativação do plugin.
- `DPS_Stock_Addon::ensure_roles_have_capability()` — Concede capability 'dps_manage_stock' para roles administrator e dps_reception.
- `DPS_Stock_Addon::dps_subscription_check_base_plugin()` — Verifica se o plugin base DPS está ativo.
- `DPS_Stock_Addon::dps_subscription_load_textdomain()` — Carrega arquivos de tradução para o subscription add-on.

## Subscription Add-on

### Funções globais

- `dps_subscription_check_base_plugin()` — Verifica se o plugin base DPS está ativo.
- `dps_subscription_load_textdomain()` — Carrega arquivos de tradução para o subscription add-on.

### APIs (classes e métodos públicos)

#### DPS_Subscription_Addon
*Classe principal de implementação do add-on de assinaturas.*
- `DPS_Subscription_Addon::enqueue_assets()` — Enfileira assets CSS/JS e localiza strings i18n para UI de gestão de assinaturas.
- `DPS_Subscription_Addon::register_subscription_cpt()` — Registra custom post type 'dps_subscription' para armazenar dados de assinaturas.
- `DPS_Subscription_Addon::add_subscriptions_tab()` — Adiciona aba de navegação "Assinaturas" à UI do plugin base.
- `DPS_Subscription_Addon::add_subscriptions_section()` — Renderiza conteúdo da seção de assinaturas na UI do plugin base.
- `DPS_Subscription_Addon::maybe_handle_subscription_request()` — Processa todas as ações de assinatura: save, cancel, restore, delete, renew e atualizações de status de pagamento (com validação de nonce).
- `DPS_Subscription_Addon::handle_subscription_payment_status()` — Atualiza status de pagamento de assinatura desde gateway de pagamento externo; sincroniza com módulo financeiro.
- `DPS_Subscription_Addon::maybe_sync_finance_on_save()` — Sincroniza registros financeiros de assinatura após operações de salvamento.


---

## Notas de cobertura

- Este catálogo lista **todas as funções/métodos documentados** no arquivo de referência, além de algumas APIs citadas apenas no índice (sem detalhamento de métodos).

- Para classes citadas sem lista de métodos, recomenda-se confirmar as assinaturas no código atual antes de finalizar a paridade no DPS v2.
