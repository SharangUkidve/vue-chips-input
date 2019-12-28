# ChipsInput

## Props

<!-- @vuese:ChipsInput:props:start -->

| Name          | Description                              | Type      | Required | Default |
| ------------- | ---------------------------------------- | --------- | -------- | ------- |
| value         | array of chips for v-model binding       | `Array`   | `true`   | -       |
| disabled      | whether to disable the form field        | `Boolean` | `false`  | `false` |
| removable     | whether to remove or not to remove chips | `Boolean` | `false`  | `true`  |
| addOnBlur     | whether to add chips on blur of input    | `Boolean` | `false`  | `false` |
| splitOn       | where to split the chips                 | `String`  | `false`  | `,`     |
| maxChipLength | max length of a chip                     | `Number`  | `false`  | -       |
| minChipLength | min length of a chip                     | `Number`  | `false`  | `0`     |
| maxChips      | maximum number of chips allowed          | `Number`  | `false`  | -       |
| chipClass     | custom class for chips                   | `String`  | `false`  | -       |
| invalidClass  | custom class for invalid chips           | `String`  | `false`  | -       |

<!-- @vuese:ChipsInput:props:end -->

## Events

<!-- @vuese:ChipsInput:events:start -->

| Event Name | Description                         | Emitted Value        |
| ---------- | ----------------------------------- | -------------------- |
| add        | emits when new chips are added      | array of added chips |
| input      | emits new chips for v-model binding | updated chips model  |
| remove     | emits when a chips is removed       | removed chip         |

<!-- @vuese:ChipsInput:events:end -->

## Slots

<!-- @vuese:ChipsInput:slots:start -->

| Name      | Description                                 | Default Slot Content   |
| --------- | ------------------------------------------- | ---------------------- |
| default   | for rendering custom chip content           | `chip.value \|\| chip` |
| closeIcon | for rendering custom icon for removing chip | `<i\>&times;</i\>`     |

<!-- @vuese:ChipsInput:slots:end -->
