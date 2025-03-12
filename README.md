# DDLX
### A Postgres extension providing portable application level permissions for local-first sync

DDLX was designed for use with ElectricSQL as a built-in permissions layer:

https://legacy.electric-sql.com/docs/api/ddlx

Electric has since evolved into a simpler, scalable Postgres sync service https://electric-sql.com/ 
and no longer provides a built-in permissions system. 

This repo is work-in-progress (don't hold your breath) to take the existing DDLX and rework it as a Postgres extension, 
particularly with an eye on using it in tandem with Electric's Postgres <> PGLite sync.

When ready DDLX will be a Postgres extension suitable for writing complex application level permissions for direct database to database partial sync.

DDLX is particularly suitable for local-first architectures as it can be evaluated by any node in a system holding a sub-set of the data; 
the permission rules and the grants are all in the data itself. 

The authority over records with lies with their creators but rather than using chains of certs to enforce things like ownership and 
sharing the central db holds a common set of agreed rules and acts as a trusted proxy between users allowing for much lighter weight evaluation.

For reads it will provide row and column based filtering based on scopes and users roles.

For writes there will be three flavors of permissions grant that offer different behaviours for partitioned writes:

- `soft` These permissions are irrevocable in a partitioned system. They naturally converge and offer finality of local writes, like a permissions CRDT, but they allow certain kinds of abuse.
- `hard` These permissions are always re-evaluated when changes made under them are shared and so are open to revocation by their issuer. All writes made under them are tentative, but they allow strict central enforcement.
- `lease` These are soft permissions that turn hard after a period of time.

