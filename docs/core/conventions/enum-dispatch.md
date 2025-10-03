---
sidebar_position: 2
---

# Enum dispatch

In most production contracts you want to handle multiple message types in a single contract.
Unfortunately you can't create multiple endpoints of the same type in a single contract, so you need
to dispatch based on the message type.

The most common way to do this is to use an enum to represent the different message types and then
match on that enum in the endpoint.

```rust title="contract.rs"
#[cw_serde]
enum ExecuteMsg {
    Add { value: i32 },
    Subtract { value: i32 },
}

#[cfg_attr(not(feature = "library"), entry_point)]
pub fn execute(
    deps: DepsMut,
    env: Env,
    info: MessageInfo,
    msg: ExecuteMsg,
) -> StdResult<Response> {
    match msg {
        ExecuteMsg::Add { value } => {
            // TODO: Add operation
        }
        ExecuteMsg::Subtract { value } => {
            // TODO: Subtract operation
        }
    }
    Ok(Response::new())
}
```
