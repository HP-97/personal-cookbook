## Instantiate a LDAP connection

Instantiate a LDAP connection

### Rust

NOTE: This example creates an asynchronous connection to a LDAP server
```rust,edition2021,no_run
use anyhow::Result;
use ldap3::{Ldap, LdapConnAsync, Scope, SearchEntry};

pub struct LdapHelper {
    /// URL to the AD Domain Controller
    pub ldap_url: String,
    /// Bind username
    pub bind_dn: String,
    /// Bind password
    pub bind_pw: String,
}

impl LdapHelper {
    /// Returns a LdapConnAsync instance that has successfully binded on success. 
    pub async fn get_ldap_conn(&self) -> Result<Ldap> {
        let (conn, mut ldap) = LdapConnAsync::new(&self.ldap_url).await?
        ldap3::drive!(conn);
        let _res = ldap.simple_bind(&self.bind_dn, &self.bind_pw).await?
        Ok(ldap)
    }
}

pub fn main() -> Result<()> {}
```