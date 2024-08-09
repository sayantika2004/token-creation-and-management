module MyToken::TokenManager {
    use aptos_framework::coin::{Self, Coin};
    use aptos_framework::aptos_account::{Self, Signer};
    use aptos_framework::aptos_std::String;

    /// A resource representing a specific token
    struct Token has key, store {
        name: String,
        symbol: String,
        total_supply: u64,
    }

    /// The resource that holds the user's tokens
    struct TokenStore has key {
        balance: Coin<Token>,
    }

    /// Initializes a new token with the given name, symbol, and total supply
    public fun create_token(
        account: &signer,
        name: String,
        symbol: String,
        total_supply: u64,
    ) {
        let token = Token {
            name,
            symbol,
            total_supply,
        };

        let token_store = TokenStore {
            balance: Coin<Token>};

        move_to(account, token);
        move_to(account, token_store);
    }

    /// Mints new tokens and adds them to the token store of the creator
    public fun mint_tokens(
        account: &signer,
        amount: u64,
    ) {
        let token_store = borrow_global_mut<TokenStore>(Signer::address_of(account));
        Coin::mint<Token>(amount, &mut token_store.balance);

    }

    /// Transfers tokens from one account to another
    public fun transfer_tokens(
        sender: &signer,
        recipient: address,
        amount: u64,
    ) {
        let sender_store = borrow_global_mut<TokenStore>(Signer::address_of(sender));
        let recipient_store = borrow_global_mut<TokenStore>(recipient);

        Coin::withdraw<Token>(&mut sender_store.balance, amount);
        Coin::deposit<Token>(&mut recipient_store.balance, amount);

    }
}
