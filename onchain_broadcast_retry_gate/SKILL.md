# Verify a broadcast before you retry it

## Compute the hash before you send

- Sign the transaction locally and compute its hash from the signed bytes before you call the broadcast endpoint.

- Record the hash, the sender, the nonce and the intended call beside the wallet index in the workspace result file before you send anything.

- Keep the signed bytes until the chain confirms the transaction or you prove it never landed.

## Check the chain before you retry

- Treat a broadcast error as unknown until you check the chain, because the transaction may already be on chain even when the endpoint returns an error.

- Look up the precomputed hash with get_transaction and wait for the chain head to pass the expected block before you call the result missing.

- Resume the batch from the confirmed transaction when the lookup finds it, and move to the next wallet without sending it again.

- Send the transaction again only after the lookup and a nonce check both show it never landed, and reuse the same nonce when you resend it.

## Treat a stale read as unverified

- Treat a missing lookup as unverified when the RPC lags behind the chain head, and recheck after a short wait instead of declaring the transaction lost.

- Compare the on chain nonce for the sender with the transaction nonce before you decide a nonce too low error is real.

- Treat a nonce too low error as proof the original transaction landed when the on chain nonce already passed it, and stop retrying that transaction.

## Keep the batch moving safely

- Log every swallowed send with its hash, wallet index and recovery action in the batch result file, so the next run can tell a real failure from a lost response.

- Pause the batch after the second swallowed send on the same endpoint and switch RPC endpoints before you continue, instead of retrying blindly through the same failing endpoint.

- Report the batch as complete only after every wallet has a confirmed transaction or a proven never landed result, and name any wallet left unresolved.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Send through a second RPC endpoint for the confirmation lookup when the broadcast endpoint stops answering, so a slow endpoint cannot hide a landed transaction.

- Split a large batch into smaller groups with a checkpoint file per group, so one swallowed send stalls only its own group.
