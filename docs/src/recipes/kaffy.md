# Kaffy - 최소한의 노력으로 관리자 화면 구축하기

![Kaffy Screenshot](https://github.com/aesmail/kaffy/blob/master/demos/kaffy_index.png?raw=true)

[kaffy 공식 페이지](https://github.com/aesmail/kaffy)

Kaffy는 기존의 코드베이스를 크게 건드리지 않고도 여러분이 필요로 하는 관리자 화면을 빠르게 개발할 수 있도록 도와주는 라이브러리입니다. 20줄 내외의 코드만으로 어렵지 않게 관리자 화면을 구축할 수 있어요.

## Kaffy 사용해보기

**STEP 1** : `mix.exs` 파일을 열어 kaffy 의존성을 추가해줍니다.

```elixir
def deps do
  [
    {:kaffy, "~> 0.9.0"}
  ]
end
```

**STEP 2** : `config.exs` 파일을 열어 `kaffy`를 사용하는데 필요한 정보를 선언해둡니다.

```elixir
config :kaffy,
  otp_app: :my_app,
  ecto_repo: MyApp.Repo,
  router: MyAppWeb.Router
```

**STEP 3** : `endpoint.exs` 파일을 열어 `kaffy`에서 제공하는 정적 리소스를 로딩할 수 있도록 해줍니다.

```elixir
plug Plug.Static,
  at: "/kaffy",
  from: :kaffy,
  gzip: false,
  only: ~w(assets)
```

**STEP 4** : `router.exs` 파일을 열어 관리자 화면으로 향하는 경로를 설정해줍니다. <br/>
> `pipe_through`를 통해 관리자 화면에 대한 접근제어도 가능해요.

```elixir
  use Kaffy.Routes, scope: "/admin", pipe_through: [:kaffy_browser, :protected]

  ...

  pipeline :protected do
    plug Pow.Plug.RequireAuthenticated,
      error_handler: Pow.Phoenix.PlugErrorHandler
  end
```

위에서 설명한 4번의 작업만으로도 관리자 화면을 쉽고 빠르게 구축할 수 있습니다.