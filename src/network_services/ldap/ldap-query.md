## Query objects from a LDAP Server

_Assume you are using the language's corresponding LDAP instantiation code from LDAP_

### Rust

```rust,edition2021,no_run
use anyhow::Result;
use serde::{Deserialize, Serialize};

#[derive(Deserialize, Debug, Default, Clone, Serialize)]
pub struct LdapUser {
    #[serde(rename = "distinguishedName")]
    pub dn: String
    #[serde(rename = "sAMAccountName")]
    pub sam_account_name: String,

}

impl LdapUser {
    /// Create a new LdapUser instance based off the hashmap output from LDAP
    fn new(&self, hashmap: HashMap<String, Vec<String>>) -> Result<Self> {
        let mut ret_hashmap: HashMap<String, String> = HashMap::new(); 

        let Ok(attrs) = TestStruct::get_ldap_user_attrs() else {
            panic!("failed to get ldap user attributes");
        };

        // Iterate through the attributes and check if they are defined in the LDAP hashmap
        // 
        // If a value is found, join the vector with ", " to ensure it becomes a string 
        // Otherwise, use an empty string
        for attr in attrs {
            match hashmap.get(&attr) {
                Some(v) => {
                    ret_hashmap.entry(attr).or_insert(v.join(", "))
                },
                // default to empty string
                None => ret_hashmap.entry(attr).or_insert("".into()),
            };
        }

        let business_category = ret_hashmap.get("businessCategory").unwrap_or(&"".into()).to_string();
        let company = ret_hashmap.get("company").unwrap_or(&"".into()).to_string();
        Ok(LdapUser { business_category, company })
    }

    fn get_ldap_user_attrs() -> Result<Vec<String>> {
        let v = LdapUser::default();
        let iterable_attrs: HashMap<String, String> = serde_json::from_value(serde_json::to_value(&v)?)?;

        Ok(iterable_attrs.into_iter().map(|v| v.0).collect::<Vec<String>>())
    }
}

impl LdapHelper {
    /// Return all user objects in AD given a base DN
    pub async fn get_all_user_objects(&self) -> Result<Vec<>> {
        let mut ldap_users: Vec<LdapUser>
    }
}
```

If your code retrieves value from a hashmap often, there is a fairly likely chance that you will be typing the LDAP attributes out multiple times.

Here is a way you can define the LDAP attributes within an enum for reuse

```rust,edition2021,no_run
use strum::IntoEnumIterator;
use strum_macros::{Display, EnumIter, EnumString};

#[derive(Display, Debug, EnumIter, EnumString)]
pub enum LdapAttribute {
    #[strum(serialize = "businessCategory")]
    BusinessCategory(String),
    #[strum(serialize = "company")]
    Company(String),
    #[strum(serialize = "department")]
    Department(String),
    #[strum(serialize = "description")]
    Description(String),
    #[strum(serialize = "displayName")]
    DisplayName(String),
    #[strum(serialize = "distinguishedName")]
    DN(String),
    #[strum(serialize = "employeeNumber")]
    EmployeeNumber(String), 
    #[strum(serialize = "givenName")]
    GivenName(String),
    #[strum(serialize = "lastLogonTimestamp")]
    LastLogonTimestamp(String),
    #[strum(serialize = "memberOf")]
    MemberOf(Vec<String>),
    #[strum(serialize = "objectCategory")]
    ObjectCategory(Vec<String>),
    #[strum(serialize = "objectClass")]
    ObjectClass(Vec<String>),
    #[strum(serialize = "sAMAccountName")]
    SamAccountName(String),
    #[strum(serialize = "sn")]
    SN(String),
    #[strum(serialize = "title")]
    Title(String),
    #[strum(serialize = "userAccountControl")]
    UserAccountControl(String),
    #[strum(serialize = "userPrincipalName")]
    UserPrincipalName(String)
}

```