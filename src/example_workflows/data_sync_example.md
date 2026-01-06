# WordPress → Supabase Sync Pseudocode

1. New user registered in WordPress
2. Trigger REST API call to Supabase sync endpoint
3. Validate payload (user ID, plan, contact info)
4. Insert/update user in Supabase users table
5. Return success/failure to WordPress
6. Log sync attempt for auditing
