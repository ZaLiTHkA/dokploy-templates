1. Open the generated domain. On first visit Campfire runs a setup wizard to create the admin account. The admin email is shown on the sign-in page as a support contact.
   **Complete the wizard immediately after the first deploy:** until an admin account exists, anyone who reaches the URL can claim it. Afterwards, new users can only join via the invite link (Account settings); regenerate it if it leaks.
2. All data (SQLite database and uploaded files) lives in the `campfire-storage` volume mounted at `/rails/storage`. Back it up regularly; run `script/admin/prepare-backup` inside the container first to get a consistent database snapshot in `storage/backups/`.
3. Web Push VAPID keys are generated automatically on first boot and saved to `storage/vapid.env`. To use your own, set `VAPID_PRIVATE_KEY` and `VAPID_PUBLIC_KEY` (generate with `script/admin/generate-secrets`). Changing them later invalidates existing push subscriptions.
4. Do not change `SECRET_KEY_BASE` after setup; it invalidates sessions and signed @mentions.
5. `DISABLE_SSL=true` is required because Dokploy's proxy terminates TLS. Push notifications need the site to be served over HTTPS.
