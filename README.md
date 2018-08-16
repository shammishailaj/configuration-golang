# golang-pkg

[![pipeline status](https://gitlab.teamc.io/teamc.io/microservice/configuration/golang-pkg/badges/master/pipeline.svg)](https://gitlab.teamc.io/teamc.io/microservice/configuration/golang-pkg/commits/master) [![coverage report](https://gitlab.teamc.io/teamc.io/microservice/configuration/golang-pkg/badges/master/coverage.svg)](https://gitlab.teamc.io/teamc.io/microservice/configuration/golang-pkg/commits/master)

Читатель yaml конфигураций по спецификации https://confluence.teamc.io/pages/viewpage.action?pageId=4227704

## Пример использования

```go
// Пакет по работе с конфигом в приложении
package config

import (
	"gitlab.teamc.io/teamc.io/microservice/configuration/golang-pkg"
	"gopkg.in/yaml.v2"
)

// Хранилище конфига приложения
var Config *confStruct

// Структура конфига. По этой структуре будет заполняться конфиг из файла. Всё, что будет 
// лишнее в yaml, в структуру не зайдёт!
type confStruct struct {
	Log struct {
		Level  string `yaml:"level"`
		Debug  bool   `yaml:"debug"`
		Format string `yaml:"yaml"`
	} `yaml:"log"`
	Database struct {
		Host           string `yaml:"host"`
		Port           string `yaml:"port"`
		User           string `yaml:"user"`
		Pass           string `yaml:"password"`
		Name           string `yaml:"name"`
		SslMode        string `yaml:"sslMode"`
		Logs           bool   `yaml:"logs"`
		MigrateOnStart bool   `yaml:"migrateOnStart"`
		MigrationPath  string `yaml:"migrationsPath"`
	} `yaml:"db"`
	Http struct {
		Host string `yaml:"host"`
		Port string `yaml:"port"`
	} `yaml:"ws"`
}

func InitConfig() error {
	// Читаем конфиг из папки. STAGE передаётся в env. Папка конфига, если переопределяется,
	// передаётся во флагах CLI
	configBytes, err := config.ReadConfigs()
	if err != nil {
		return err
	}

    // Успешно прочитав файл и получив слайс байтов, отдаём их на анмаршаллинг в структуру
    // конфига приложения. 
	return yaml.Unmarshal(configBytes, &Config)
} 
```