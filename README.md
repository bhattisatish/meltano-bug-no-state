# Intro

This repo is a sample repo to replicate a bug / issue with meltano while running it in docker where we try configure it to use postgres db for state management.

# To Setup locally

1. Ensure docker and docker compose extension are installed in your machine.
2. From the root of the repo, initialize .meltano folder by running `./init.sh` 
3. From the root of the repo, run `docker compose build`
4. Try bringing up the services by `docker compose up`

It should fail with the following exception:
```
meltano-1            | 2025-05-23T11:44:47.946085Z [error    ] No such revision or branch '5f2621c13b39'
meltano-1            | ╭───────────────────── Traceback (most recent call last) ──────────────────────╮
meltano-1            | │ /venv/lib/python3.9/site-packages/meltano/core/migration_service.py:100 in   │
meltano-1            | │ upgrade                                                                      │
meltano-1            | │                                                                              │
meltano-1            | │    97 │   │   │   try:                                                       │
meltano-1            | │    98 │   │   │   │   # try to find the locked version                       │
meltano-1            | │    99 │   │   │   │   head = LOCK_PATH.read_text().strip()                   │
meltano-1            | │ ❱ 100 │   │   │   │   self.ensure_migration_needed(script, context, head)    │
meltano-1            | │   101 │   │   │   │                                                          │
meltano-1            | │   102 │   │   │   │   if not silent:                                         │
meltano-1            | │   103 │   │   │   │   │   click.secho(f"Upgrading database to {head}")       │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/meltano/core/migration_service.py:59 in    │
meltano-1            | │ ensure_migration_needed                                                      │
meltano-1            | │                                                                              │
meltano-1            | │    56 │   │   """                                                            │
meltano-1            | │    57 │   │   current_head = context.get_current_revision()                  │
meltano-1            | │    58 │   │                                                                  │
meltano-1            | │ ❱  59 │   │   for rev in script.iterate_revisions(current_head, "base"):     │
meltano-1            | │    60 │   │   │   if rev.revision == target_revision:                        │
meltano-1            | │    61 │   │   │   │   raise MigrationUneededException                        │
meltano-1            | │    62                                                                        │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:814 in          │
meltano-1            | │ iterate_revisions                                                            │
meltano-1            | │                                                                              │
meltano-1            | │    811 │   │   else:                                                         │
meltano-1            | │    812 │   │   │   fn = self._collect_upgrade_revisions                      │
meltano-1            | │    813 │   │                                                                 │
meltano-1            | │ ❱  814 │   │   revisions, heads = fn(                                        │
meltano-1            | │    815 │   │   │   upper,                                                    │
meltano-1            | │    816 │   │   │   lower,                                                    │
meltano-1            | │    817 │   │   │   inclusive=inclusive,                                      │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:1447 in         │
meltano-1            | │ _collect_upgrade_revisions                                                   │
meltano-1            | │                                                                              │
meltano-1            | │   1444 │   │   """                                                           │
meltano-1            | │   1445 │   │   targets: Collection[Revision] = [                             │
meltano-1            | │   1446 │   │   │   is_revision(rev)                                          │
meltano-1            | │ ❱ 1447 │   │   │   for rev in self._parse_upgrade_target(                    │
meltano-1            | │   1448 │   │   │   │   current_revisions=lower,                              │
meltano-1            | │   1449 │   │   │   │   target=upper,                                         │
meltano-1            | │   1450 │   │   │   │   assert_relative_length=assert_relative_length,        │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:1234 in         │
meltano-1            | │ _parse_upgrade_target                                                        │
meltano-1            | │                                                                              │
meltano-1            | │   1231 │   │                                                                 │
meltano-1            | │   1232 │   │   if not match:                                                 │
meltano-1            | │   1233 │   │   │   # No relative destination, target is absolute.            │
meltano-1            | │ ❱ 1234 │   │   │   return self.get_revisions(target)                         │
meltano-1            | │   1235 │   │                                                                 │
meltano-1            | │   1236 │   │   current_revisions_tup: Union[str, Tuple[Optional[str], ...],  │
meltano-1            | │   1237 │   │   current_revisions_tup = util.to_tuple(current_revisions)      │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:565 in          │
meltano-1            | │ get_revisions                                                                │
meltano-1            | │                                                                              │
meltano-1            | │    562 │   │   │   │   except ValueError:                                    │
meltano-1            | │    563 │   │   │   │   │   # couldn't resolve as integer                     │
meltano-1            | │    564 │   │   │   │   │   pass                                              │
meltano-1            | │ ❱  565 │   │   │   return tuple(                                             │
meltano-1            | │    566 │   │   │   │   self._revision_for_ident(rev_id, branch_label)        │
meltano-1            | │    567 │   │   │   │   for rev_id in resolved_id                             │
meltano-1            | │    568 │   │   │   )                                                         │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:566 in          │
meltano-1            | │ <genexpr>                                                                    │
meltano-1            | │                                                                              │
meltano-1            | │    563 │   │   │   │   │   # couldn't resolve as integer                     │
meltano-1            | │    564 │   │   │   │   │   pass                                              │
meltano-1            | │    565 │   │   │   return tuple(                                             │
meltano-1            | │ ❱  566 │   │   │   │   self._revision_for_ident(rev_id, branch_label)        │
meltano-1            | │    567 │   │   │   │   for rev_id in resolved_id                             │
meltano-1            | │    568 │   │   │   )                                                         │
meltano-1            | │    569                                                                       │
meltano-1            | │                                                                              │
meltano-1            | │ /venv/lib/python3.9/site-packages/alembic/script/revision.py:637 in          │
meltano-1            | │ _revision_for_ident                                                          │
meltano-1            | │                                                                              │
meltano-1            | │    634 │   │   │   if branch_rev:                                            │
meltano-1            | │    635 │   │   │   │   revs = self.filter_for_lineage(revs, check_branch)    │
meltano-1            | │    636 │   │   │   if not revs:                                              │
meltano-1            | │ ❱  637 │   │   │   │   raise ResolutionError(                                │
meltano-1            | │    638 │   │   │   │   │   "No such revision or branch '%s'%s"               │
meltano-1            | │    639 │   │   │   │   │   % (                                               │
meltano-1            | │    640 │   │   │   │   │   │   resolved_id,                                  │
meltano-1            | ╰──────────────────────────────────────────────────────────────────────────────╯
meltano-1            | ResolutionError: No such revision or branch '5f2621c13b39'
meltano-1            | 
meltano-1            | Need help fixing this problem? Visit http://melta.no/ for troubleshooting steps, or to
meltano-1            | join our friendly Slack community.
meltano-1            | 
meltano-1            | Cannot upgrade the system database. It might be corrupted or was created before database migrations where introduced (v0.34.0)
```

If you comment out `MELTANO_DATABASE_URI` env variable in [docker-compose.yml](./docker-compose.yml) and re-run `docker compose up` you should see all the services are up.

