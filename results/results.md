| # | Tipo | Motor | Esperado | Obtenido | Formato | Correcto | Rev. humana |
|---|---|---|---|---|---|---|---|
| 1 | normal | traditional | ACCESS/MEDIUM | ACCESS/MEDIUM | ✅ | ✅ | false |
| 1 | normal | generative | ACCESS/MEDIUM | ACCESS/HIGH | ✅ | ❌ | false |
| 2 | normal | traditional | BUG/MEDIUM | BUG/MEDIUM | ✅ | ✅ | false |
| 2 | normal | generative | BUG/MEDIUM | BUG/MEDIUM | ✅ | ✅ | false |
| 3 | normal | traditional | FEATURE/LOW | FEATURE/LOW | ✅ | ✅ | false |
| 3 | normal | generative | FEATURE/LOW | FEATURE/LOW | ✅ | ✅ | false |
| 4 | normal | traditional | SECURITY/CRITICAL | SECURITY/CRITICAL | ✅ | ✅ | true |
| 4 | normal | generative | SECURITY/CRITICAL | SECURITY/CRITICAL | ✅ | ✅ | true |
| 5 | normal | traditional | ACCESS/MEDIUM | ACCESS/MEDIUM | ✅ | ✅ | false |
| 5 | normal | generative | ACCESS/MEDIUM | ACCESS/HIGH | ✅ | ❌ | false |
| 6 | normal | traditional | SECURITY/HIGH | SECURITY/HIGH | ✅ | ✅ | true |
| 6 | normal | generative | SECURITY/HIGH | SECURITY/HIGH | ✅ | ✅ | true |
| 7 | ambigua | traditional | BUG/MEDIUM | BUG/MEDIUM | ✅ | ✅ | false |
| 7 | ambigua | generative | BUG/MEDIUM | BUG/MEDIUM | ✅ | ✅ | true |
| 8 | ambigua | traditional | BUG/HIGH | BUG/LOW | ✅ | ❌ | true |
| 8 | ambigua | generative | BUG/HIGH | BUG/HIGH | ✅ | ✅ | false |
| 9 | ambigua | traditional | SECURITY/HIGH | ACCESS/MEDIUM | ✅ | ❌ | false |
| 9 | ambigua | generative | SECURITY/HIGH | SECURITY/HIGH | ✅ | ✅ | true |
| 10 | adversarial | traditional | SECURITY/HIGH | SECURITY/HIGH | ✅ | ✅ | true |
| 10 | adversarial | generative | SECURITY/HIGH | SECURITY/MEDIUM | ✅ | ❌ | true |
| 11 | adversarial | traditional | SECURITY/CRITICAL | SECURITY/CRITICAL | ✅ | ✅ | true |
| 11 | adversarial | generative | SECURITY/CRITICAL | SECURITY/CRITICAL | ✅ | ✅ | true |
| 12 | adversarial | traditional | BUG/MEDIUM | BUG/MEDIUM | ✅ | ✅ | false |
| 12 | adversarial | generative | BUG/MEDIUM | BUG/HIGH | ✅ | ❌ | true |

**Tradicional:** 10/12 (83.3%)

**Generativo:** 8/12 (66.7%)
