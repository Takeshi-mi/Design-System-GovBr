# Melhor Prática para Implementar o Design System do Governo (BRGOV-DS) no Laravel

Olá pessoal,

Sou iniciante no Laravel e estou trabalhando em um projeto do SINAJUVE onde precisamos seguir o design system do governo (BRGOV-DS). A biblioteca de componentes já está instalada no nosso projeto via npm. Estou preocupado com padronização e UX, e gostaria de saber qual é a melhor prática para implementar esse design system no Laravel.

Minha ideia inicial é usar Blade Components. Eu pensei em criar componentes Blade para cada elemento do design system, utilizando comandos como `php artisan make:component buttons/br-button` e depois chamá-los no código com `<x-buttons.br-button></x-buttons.br-button>`. No entanto, não tenho certeza se essa é a melhor abordagem.

Gostaria de saber se alguém aqui já implementou o BRGOV-DS no Laravel e, se sim, como foi feito. Existe algum direcionamento ou melhores práticas a serem seguidas?

**Exemplo de Botões do Design System:**

Aqui estão alguns exemplos de botões fornecidos pelo BRGOV-DS que eu gostaria de transformar em Blade Components:

```html
<button class="br-button primary mr-3" type="button">Primário</button>
<button class="br-button secondary mr-3" type="button">Secundário</button>
<button class="br-button" type="button">Terciário</button>
<button class="br-button circle primary dark-mode mr-3" type="button" aria-label="Ícone ilustrativo">
  <i class="fas fa-city" aria-hidden="true"></i>
</button>
<button class="br-button circle secondary dark-mode mr-3" type="button" aria-label="Ícone ilustrativo">
  <i class="fas fa-city" aria-hidden="true"></i>
</button>
<button class="br-button circle dark-mode" type="button" aria-label="Ícone ilustrativo">
  <i class="fas fa-city" aria-hidden="true"></i>
</button>
```

**Implementação como Blade Component:**

Para transformar esses botões em Blade Components, imaginei algo assim:

1. **Criando o Componente Blade:**

```bash
php artisan make:component Buttons/Br-button
```

2. **Definindo o Componente Blade:**

```php
// app/View/Components/Buttons/BrButton.php
namespace App\View\Components;

use Closure;
use Illuminate\Contracts\View\View;
use Illuminate\View\Component;

class Button extends Component
{
    /**
     * Create a new component instance.
     */
    public function __construct(
        public string $type = 'button',
        public string $variant = 'primary',
        public string $ariaLabel = ''
    ){}

    /**
     * Get the view / contents that represent the component.
     */
    public function render(): View|Closure|string
    {
        return view('components.button');
    }
}
```
```
php
// resources/views/components/button/br-button.blade.php
<button 
    type="{{ $type }}" 
    class="br-button {{ $variant }} {{ $attributes->get('class') }}"
    aria-label="{{ $ariaLabel }}"
    {{ $attributes }}
>
    {{ $slot }}
</button>

</button>
```

3. **Utilizando o Componente no Blade:**

```html
<x-button class="mr-3">Primário</x-button>
<x-button type="submit" variant="secondary" class="mr-3">Secundário</x-button>
<x-button>Terciário</x-button>
<x-button variant="circle primary dark-mode" ariaLabel="Ícone ilustrativo" class="mr-3">
  <i class="fas fa-city" aria-hidden="true"></i>
</x-button>
<x-button variant="circle secondary dark-mode" ariaLabel="Ícone ilustrativo" class="mr-3">
  <i class="fas fa-city" aria-hidden="true"></i>
</x-button>
<x-button variant="circle dark-mode" ariaLabel="Ícone ilustrativo">
  <i class="fas fa-city" aria-hidden="true"></i>
</x-button>
```

### Variáveis:
{{ $slot }}: permite inserir o conteúdo do botão.
variant: Para aplicar diferentes estilos de botões (e.g., primary, secondary).
type: Para definir o tipo de botão (e.g., button, submit).
ariaLabel: Para fornecer uma descrição acessível do botão, especialmente útil para botões com ícones.


**Dúvidas:**

1. Essa abordagem usando Blade Components é recomendada para implementar o BRGOV-DS no Laravel?
2. Existem melhores práticas ou outras sugestões para integrar o design system de forma mais eficiente?
3. Como posso melhorar a personalização e reutilização desses componentes?

Agradeço desde já pela ajuda e pelas sugestões!


ps: Deixei no github porque não coube no fórum do discord com os exemplos. Lá na minha pergunta eu referencio esse repositório.
