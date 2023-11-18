# Learning Config Management On Golang Using VIPER

## Golang Viper

- Viper adalah salah satu Library untuk Golang yang populer digunakan untuk
  manajemen konfigurasi.
- Viper mendukung banyak jenis file konfigurasi, seperti JSON, YAML, env file,
  properties, dan lainnya.

## Membuat Project

Sebelum belajar Viper, pertama buat project terlebih dahulu. Buat sebuah folder
baru misalnya **_GOLANG-VIPER_**, kemudian masuk ke direktori folder tersebut
lalu lakukan inisiasi project golang dengan mengetikkan.

```bash
go mod init golang-viper
```

## Menambahkan Library Viper

Untuk menambahkan library/modul ke dalam project kita, caranya masuk ke terminal
dari dalam direktori project kita. Kemudian ketikkan:

```bash
go get github.com/spf13/viper
```

karena nanti kita belajarnya sambil menggunakan unit test, maka kita tambahkan
juga library testify untuk unit testnya.

```bash
go get github.com/stretchr/testify
```

## Membuat Viper

Untuk membuat Viper, caranya cukup mudah yaitu kita bisa menggunakan function
viper.New() . Setelah membuat Viper, kita bisa menentukan dari mana kita akan
mengambil konfigurasi.

## Mulai Koding

Masuk kembali ke dalam project, kemudian tambahkan sebuah file dengan nama
_viper_test.go_. Buka file tersebut dengan text editor atau IDE favorit
teman-teman.

```go
package golangviper

import (
        "testing"

        "github.com/spf13/viper"
        "github.com/stretchr/testify/assert"
)

func TestViper(t *testing.T) {
    var config *viper.Viper = viper.New()
    assert.NotNil(t, config)
}
```

Ok kita berhasil membuat objek viper.

## JSON

Sekarang kita akan belajar, membaca konfigurasi dari file JSON. buat sebuah file
baru dengan nama _config.json_.

- file _config.json_

```json
{
  "app": {
    "name": "Belajar Golang Viper",
    "version": "1.0.0",
    "author": "Mazufik"
  },
  "database": {
    "show_sql": true,
    "host": "localhost",
    "port": 3306
  }
}
```

- file _viper_test.go_

```go
package golangviper

import (
        "testing"

        "github.com/spf13/viper"
        "github.com/stretchr/testify/assert"
)

func TestJSON(t *testing.T) {
    config := viper.New()
    config.SetConfigName("config")
    config.SetConfigType("json")
    config.AddConfigPath(".")

    // read config
    err := config.ReadInConfig()
    assert.Nil(t, err)

    assert.Equal(t, "Belajar Golang Viper", config.GetString("app.name"))
    assert.Equal(t, "Mazufik", config.GetString("app.author"))
    assert.Equal(t, "localhost", config.GetString("database.host"))
    assert.Equal(t, 3306, config.GetInt("database.port"))
    assert.Equal(t, true, config.GetBool("database.show_sql"))
}
```

## YAML

Untuk mmebaca konfigurasi dari file YAML, itu sama degan membaca konfigurasi
diJSON cuman beda format filenya saja. BUat file dengan nama _config.yaml_

- file _config.yaml_

```yaml
app:
  name: "Belajar Golang Viper"
  author: "Mazufik"
  version: "1.0.0"
database:
  host: "localhost"
  port: 3306
  show_sql: true
```

- file _viper_test.go_

```go
func TestYAML(t *testing.T) {
 config := viper.New()
 // config.SetConfigName("config")
 // config.SetConfigType("json") atau
 config.SetConfigFile("config.yaml")
 config.AddConfigPath(".")

 // read config
 err := config.ReadInConfig()
 assert.Nil(t, err)

 assert.Equal(t, "Belajar Golang Viper", config.GetString("app.name"))
 assert.Equal(t, "Mazufik", config.GetString("app.author"))
 assert.Equal(t, "localhost", config.GetString("database.host"))
 assert.Equal(t, 3306, config.GetInt("database.port"))
 assert.Equal(t, true, config.GetBool("database.show_sql"))
}
```

## ENV File

Viper juga bisa digunakan untuk membaca file dengan format ENV. Buat file dengan
nama _config.env_

- file _config.env_

```env
APP_NAME=Belajar Golang Viper
APP_AUTHRO=Mazufik
APP_VERSION=1.0.0

DATABASE_HOST=localhost
DATABASE_PORT=3306
DATABASE_SHOW_SQL=true
```

- file _viper_test.go_

```go
func TestENV(t *testing.T) {
    config := viper.New()
    config.SetConfigFile("config.env")
    config.AddConfigPath(".")

    // read config
    err := config.ReadInConfig()
    assert.Nil(t, err)

    assert.Equal(t, "Belajar Golang Viper", config.GetString("APP_NAME"))
    assert.Equal(t, "Mazufik", config.GetString("APP_AUTHOR"))
    assert.Equal(t, "localhost", config.GetString("DATABASE_HOST"))
    assert.Equal(t, 3306, config.GetInt("DATABASE_PORT"))
    assert.Equal(t, true, config.GetBool("DATABASE_SHOW_SQL"))
}
```
