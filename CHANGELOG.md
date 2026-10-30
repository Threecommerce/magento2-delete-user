# Changelog

Tutte le modifiche rilevanti a questo modulo sono documentate in questo file.

Il formato è basato su [Keep a Changelog](https://keepachangelog.com/it/1.0.0/)
e il progetto aderisce al [Versionamento Semantico](https://semver.org/lang/it/).

## [1.1.3] - 2026-10-28

### Fixed
- L'eliminazione dell'account falliva ancora dopo la cancellazione del cliente: `logout()` scriveva `customer_log` con `customer_id` nullo (violazione di integrità). La sessione ora viene chiusa prima della cancellazione.

## [1.1.2] - 2026-10-27

### Fixed
- L'eliminazione dell'account falliva sul frontend ("Cannot complete this operation from non-admin area"): la cancellazione di cliente, iscrizione newsletter e ordini ora avviene in area sicura (`isSecureArea`), limitata a quelle operazioni. Rimosso il campo `version` da `composer.json`: la versione deriva dal tag (il tag 1.1.1 non è mai stato pubblicato).

## [1.1.0] - 2026-09-04

### Added
- Cancellazione dell'iscrizione alla newsletter contestuale all'eliminazione
  dell'account (`Model\NewsletterSubscriptionRemover`), con opzione admin
  **Data Removal > Remove Newsletter Subscription** (attiva di default). Vengono
  rimosse le righe collegate all'id cliente e all'indirizzo email su tutti i
  website.
- Opzione admin **Data Removal > Delete Orders, Invoices and Credit Memos**
  (disattiva di default) che elimina ordini, fatture, spedizioni, note di
  credito e le relative righe delle tabelle grid (`Model\SalesDataRemover`).
  Include gli ordini guest con lo stesso indirizzo email.
- Email transazionale di conferma eliminazione (`Model\AccountDeletedEmailSender`)
  con gruppo di configurazione **Deletion Confirmation Email** (abilitazione,
  mittente e template selezionabile) e template di default
  `view/frontend/email/account_deleted.html` registrato in `etc/email_templates.xml`.
- Traduzioni italiane per le nuove label, i commenti admin e i testi dell'email.

### Changed
- `Controller\Account\Delete` raccoglie gli id dei record correlati prima di
  eliminare il cliente e li rimuove solo dopo l'esito positivo; gli errori delle
  operazioni accessorie vengono loggati senza bloccare il flusso.
- Dipendenze modulo/composer estese a `Magento_Newsletter`, `Magento_Sales` e
  `Magento_Email`.

## [1.0.0] - 2026-07-01

### Added
- Link "Delete Account" nella navigazione dell'area My Account (compatibile Luma / Hyva / Breeze tramite il blocco nativo `customer_account_navigation`).
- Pagina di conferma con form HTML puro (nessun widget Luma) e ViewModel `DeleteConfig`.
- Controller `Account\Index` (visualizza il form) e `Account\Delete` (esegue l'eliminazione via `CustomerRepositoryInterface`).
- Conferma tramite reinserimento della password (`AuthenticationInterface`) configurabile e checkbox di conferma esplicita.
- Protezione CSRF tramite `form_key`; logout e rigenerazione della sessione dopo l'eliminazione.
- Configurazione admin in **Stores > Configuration > Threecommerce Plugins > Delete Account** (abilitazione e richiesta password).
- Testi in inglese con traduzioni in `i18n/it_IT.csv`.