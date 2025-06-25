<!-- use vrs_core_sdk::{
    export, get, init, io::_eprint, io::_print, set_timer, timer, timer::now,
    tss::tss_get_public_key, tss::tss_sign, tss::CryptoType,
};
#[export]
pub struct A {
    a: u8,
    b: u8,
    c: u8,
}
#[init]
pub fn init() {
    set_timer!(std::time::Duration::from_secs(1), ttt,);
    set_timer!(std::time::Duration::from_secs(1), ttt,);
    set_timer!(std::time::Duration::from_secs(1), ttt,);
}
#[timer]
pub fn ttt() {
    _print(format!("now: {:?}", now()));
    match tss_get_public_key(CryptoType::EcdsaSecp256k1, vec![1, 2, 3]) {
        Ok(r) => {
            _print(format!("public key: {:?}", hex::encode(r)));
        }
        Err(e) => {
            _eprint(format!("{:?}", e));
        }
    }
    match tss_get_public_key(CryptoType::EcdsaSecp256k1, vec![2, 2, 3]) {
        Ok(r) => {
            _print(format!("public key: {:?}", hex::encode(r)));
        }
        Err(e) => {
            _eprint(format!("{:?}", e));
        }
    }
    match tss_get_public_key(CryptoType::EcdsaSecp256k1, vec![1, 2, 3]) {
        Ok(r) => {
            _print(format!("public key: {:?}", hex::encode(r)));
        }
        Err(e) => {
            _eprint(format!("{:?}", e));
        }
    }
    match tss_sign(CryptoType::EcdsaSecp256k1, vec![1, 2, 3], vec![12; 32]) {
        Ok(r) => {
            _print(format!("sign: {:?}", hex::encode(r)));
        }
        Err(e) => {
            _eprint(format!("sign error: {:?}", e));
        }
    }
    set_timer!(std::time::Duration::from_secs(1), ttt,);
}
#[get]
pub fn get_public_key(crypto_type: CryptoType, tweak: Vec<u8>) -> Result<Vec<u8>, String> {
    tss_get_public_key(crypto_type, tweak).map_err(|e| e.to_string())
}
#[get]
pub fn sign(crypto_type: CryptoType, tweak: Vec<u8>, message: Vec<u8>) -> Result<Vec<u8>, String> {
    tss_sign(crypto_type, tweak, message).map_err(|e| e.to_string())
} -->
