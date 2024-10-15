# Ex 3

| Source     | Port | Dest       | Port | Protocle | Paquet Syn | Action    |
| ---------- | ---- | ---------- | ---- | -------- | ---------- | --------- |
| *          | *    | ip interne | 25   | tcp      | *          | authorise |
| *          | 25   | ip interne | *    | tcp      | no         | authorise |
| ip interne | *    | *          | 25   | tcp      | *          | authorise |
| ip interne | 25   | *          | *    | tcp      | no         | authorise |
| *          | *    | *          | *    | *        | *          | bloc, log |
