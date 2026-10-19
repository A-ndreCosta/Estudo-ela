# Estudo

## Sincronização partilhada (Firestore)

Os dados deste site ficam no documento `shared/state-ela`. Quem entrar com um
dos emails autorizados vê e edita os mesmos dados.

Regras a colar em **Firebase → Firestore Database → Regras** (substitui
`O_TEU_EMAIL@gmail.com` pelo email com que já entras; as regras de `users`
mantêm o outro site a funcionar):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // dados pessoais (site Estudo e dados antigos deste site)
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    // dados partilhados do Estudo-ela
    match /shared/state-ela {
      allow read, write: if request.auth != null
        && request.auth.token.email_verified == true
        && request.auth.token.email in ['O_TEU_EMAIL@gmail.com', '99inesferreira@gmail.com'];
    }
  }
}
```

Para acrescentar mais pessoas, junta o email à lista.
