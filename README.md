# HW-Own_Providers

# 7.6. Написание собственных провайдеров для Terraform.

##  Задача 1

### Вопрос 1.

перечислены все доступные resource

с 709 строки в файле provider.go

https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go

https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go#L709

перечислены все доступные data_source

с 338 строки в файле provider.go

https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go

https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go#L338

##  Задача 2.

Использовал информацию в файле internal/service/sqs/queue.go

из package sqs

### Вопрос 1.

С каким другим параметром конфликтует name? Приложите строчку кода, в которой это указано.

name_prefix  -- ConflictsWith: []string{"name_prefix"}

    92      "name": {
          Type:          schema.TypeString,
          Optional:      true,
          Computed:      true,
          ForceNew:      true,
          ConflictsWith: []string{"name_prefix"},



### Вопрос 2.

Какому регулярному выражению должно подчиняться имя?

a-zA-Z0-9_-

могут использоваться данные символы в имени

Какая максимальная длина имени?

или от 1 до 75 символов в случае fifoQueue

или от 1 до 80 символов в других случаях

    400   var name string

        if fifoQueue {
          name = create.NameWithSuffix(diff.Get("name").(string), diff.Get("name_prefix").(string), FIFOQueueNameSuffix)
        } else {
          name = create.Name(diff.Get("name").(string), diff.Get("name_prefix").(string))
        }

        var re *regexp.Regexp

        if fifoQueue {
          re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,75}\.fifo$`)
        } else {
          re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,80}$`)
        }

        if !re.MatchString(name) {
          return fmt.Errorf("invalid queue name: %s", name)
        }










