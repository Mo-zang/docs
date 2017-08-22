# Upgrade Guide
# 업그레이드 가이드

- [Upgrading To 5.5.0 From 5.4](#upgrade-5.5.0)
- [5.4에서 5.5.0 으로 업그레이드 하기](#upgrade-5.4.0)

<a name="upgrade-5.5.0"></a>
## Upgrading To 5.5.0 From 5.4
## 5.4에서 5.5.0으로 업그레이드 하기

#### Estimated Upgrade Time: 1 Hour
#### 업그레이드 예상 시간 : 약 1시간

> {note} We attempt to document every possible breaking change. Since some of these breaking changes are in obscure parts of the framework only a portion of these changes may actually affect your application.

> {note} 가능한 모든 변경 내용을 기록하려고 했습니다. 그렇지만 변경 사항 중 일부는 프레임워크의 명확하지 않은 부분에서 이루어 지기 때문에 이중 일부가 실제 어플리케이션에 영향을 끼칠 수도 있습니다.

### Updating Dependencies
### 의존성 업데이트

Update your `laravel/framework` dependency to `5.5.*` in your `composer.json` file. In addition, you should update your `phpunit/phpunit` dependency to `~6.0`.

`composer.json` 파일에 있는 laravel/framework 의존성을 `5.5.*` 로 변경합니다. 그리고 `phpunit/phpunit`을 `~6.0` 으로 업데이트 해야 합니다.

#### Laravel Dusk
#### 라라벨 Dusk

Laravel Dusk `2.0.0` has been released to provide compatibility with Laravel 5.5 and headless Chrome testing.

라라벨 Dusk `2.0.0`이 릴리즈되어 라라벨 5.5와 Headless 크롬 테스팅의 호환성을 제공합니다.

#### Pusher
#### Pusher

The Pusher event broadcasting driver now requires version `~3.0` of the Pusher SDK.

Push 이벤트 브로드캐시팅 드라이버는 이제 Pusher SDK의 `~3.0` 릴리즈 버전을 필요로 합니다.

### Artisan
### Artisan

#### The `fire` Method
#### `fire` 메소드

Any `fire` methods present on your Artisan commands should be renamed to `handle`.

아티즌 명령어에 존재하는 `fire` 메소드는 `handle` 으로 이름을 변경해야 합니다.

### Authorization
### 권한 승인-authorization

#### The `authorizeResource` Controller Method
#### `authorizeResource` 컨트롤러 메소드

When passing a multi-word model name to the `authorizeResource` method, the resulting route segment will now be "snake" case, matching the behavior of resource controllers.

#### The `before` Policy Method
#### `before` Policy 메소드

The `before` method of a policy class will not be called if the class doesn't contain a method with name matching the name of the ability being checked.

policy 클래스의 `before` 메소드는 클래스가 검사되는 ability 이름과 일치하는 메소드가 없는 경우에 이를 호출하지 않습니다.

### Cache
### 캐시

#### Database Driver
#### 데이터베이스 드라이버

If you are using the database cache driver, you should run `php artisan cache:clear` when deploying your upgraded Laravel 5.5 application for the first time.

데이터베이스 캐시 드라이버를 사용하고 있었다면, 라라벨 5.5으로 업그레이드한 어플리케이션을 배포하고 나서 처음 한번 `php artisan cache:clear` 메소드를 실행해야 합니다.

### Eloquent
### Eloquent

#### `belongsToMany` Method
#### `belongsToMany` 메소드

If you are overriding the `belongsToMany` method on your Eloquent model, you should update your method signature to reflect the addition of new arguments:

Eloquent 모델에서 `belongsToMany` 메소드를 오버라이딩해서 사용하고 있었다면, 몇개의 추가된 인자들에 대응하기 위해서 메소드를 수정해야 합니다:

    /**
     * Define a many-to-many relationship.
     *
     * @param  string  $related
     * @param  string  $table
     * @param  string  $foreignPivotKey
     * @param  string  $relatedPivotKey
     * @param  string  $parentKey
     * @param  string  $relatedKey
     * @param  string  $relation
     * @return \Illuminate\Database\Eloquent\Relations\BelongsToMany
     */
    public function belongsToMany($related, $table = null, $foreignPivotKey = null,
                                  $relatedPivotKey = null,$parentKey = null,
                                  $relatedKey = null, $relation = null)
    {
        //
    }

#### Model `is` Method
#### 모델의 `is` 메소드

If you are overriding the `is` method of your Eloquent model, you should remove the `Model` type-hint from the method. This allows the `is` method to receive `null` as an argument:

Eloquent 모델의 `is` 메소드를 오버라이딩해서 사용하고 있었다면, 이 메소드에 `Model` 타입 힌트를 제거해야합니다. 이렇게 하면 `is` 메소드가 `null`을 인자로 받을 수 있습니다:

    /**
     * Determine if two models have the same ID and belong to the same table.
     *
     * @param  \Illuminate\Database\Eloquent\Model|null  $model
     * @return bool
     */
    public function is($model)
    {
        //
    }

#### Model `$events` Property
#### 모델의 `$events` 속성

The `$events` property on your models should be renamed to `$dispatchesEvents`. This change was made because of a high number of users needing to define an `events` relationship, which caused a conflict with the old property name.

모델의 `$events` 속성은 `$dispatchesEvents`으로 이름이 변경되었습니다. 이 변경사항은 많은 사용자들이 `event` 라는 관계를 정의하고자 하여 기존의 이름과 충돌을 유발했기 때문입니다.

#### Pivot `$parent` Property
#### 피벗-Pivot `$parent` 속성

The protected `$parent` property on the `Illuminate\Database\Eloquent\Relations\Pivot` class has been renamed to `$pivotParent`.

`Illuminate\Database\Eloquent\Relations\Pivot` 클래스의 `$parent` 속성은 `$pivotParent` 으로 이름이 변경되었습니다.

#### Relationship `create` Methods
#### Relationship-관계에서의 `create` 메소드

The `BelongsToMany`, `HasOneOrMany`, and `MorphOneOrMany` class' `create` methods have been modified to provide a default value for the `$attributes` argument. If you are overriding these methods, you should update your signatures to match the new definition:

`BelongsToMany`, `HasOneOrMany`, 그리고 `MorphOneOrMany` 클래스의 `create` 메소드는 `$attributes` 인자를 위해서 기본값을 제공하도록 변경되었습니다. 이 메소드를 오버라이딩 한경우, 변경사항에 매칭되도록 사용한 곳을 수정해야 합니다:

    public function create(array $attributes = [])
    {
        //
    }

#### Soft Deleted Models
#### Soft 삭제 메소드

When deleting a "soft deleted" model, the `exists` property on the model will remain `true`.

"soft deleted" 모델을 삭제할 때에는 모델의 `exists` 속성을 `true` 를 유지합니다.

#### `withCount` Column Formatting
#### `withCount` 컬럼의 포맷팅

When using an alias, the `withCount` method will no longer automatically append `_count` onto the resulting column name. For example, in Laravel 5.4, the following query would result in a `bar_count` column being added to the query:

alias-별칭을 사용할 때 `withCount` 메소드는 더이상 자동으로 결과 컬럼 이름에 `_count`을 붙이지 않습니다. 예를 들어, 라라벨 5.4에서는 다음 쿼리의 결과에 `bar_count` 컬럼이 추가되었습니다:

    $users = User::withCount('foo as bar')->get();

However, in Laravel 5.5, the alias will be used exactly as it is given. If you would like to append `_count` to the resulting column, you must specify that suffix when defining the alias:

하지만, 라라벨 5.5에서는 alias-별칭이 그대로 사용됩니다. 결과 컬럼에 `_count`를 붙이고자 한다면, alias-별칭을 정의할 때 접미사를 지정해야합니다:

    $users = User::withCount('foo as bar_count')->get();

### Exception Format
### Exception 포맷

In Laravel 5.5, all exceptions, including validation exceptions, are converted into HTTP responses by the exception handler. In addition, the default format for JSON validation errors has changed. The new format conforms to the following convention:

라라벨 5.5에서는 유효성 검사-validation exception-예외를 포함하여 모든 exception-예외는 exception 핸들러에 의해서 HTTP 응답-response으로 변환합니다. 또한 JSON 유효성 검사에러에 대한 기본 포맷이 변경되었습니다. 새로운 포맷은 다음 형태를 따릅니다:

    {
        "message": "The given data was invalid.",
        "errors": {
            "field-1": [
                "Error 1",
                "Error 2"
            ],
            "field-2": [
                "Error 1",
                "Error 2"
            ],
        }
    }

However, if you would like to maintain the Laravel 5.4 JSON error format, you may add the following method to your `App\Exceptions\Handler` class:

하지만, 라라벨 5.4의 JSON 에러 포맷을 유지하고자 한다면, `App\Exceptions\Handler` 클래스에 다음 메서드를 추가 하면 됩니다:

    use Illuminate\Validation\ValidationException;

    /**
     * Convert a validation exception into a JSON response.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Illuminate\Validation\ValidationException  $exception
     * @return \Illuminate\Http\JsonResponse
     */
    protected function invalidJson($request, ValidationException $exception)
    {
        return response()->json($exception->errors(), $exception->status);
    }

#### JSON Authentication Attempts
#### JSON 인증 시도

This change also affects the validation error formatting for authentication attempts made over JSON. In Laravel 5.5, JSON authentication failures will return the error messages following the new formatting convention described above.

이 변경사항은 JSON을 통한 인증 시도에서 validation-유효성 검증 에러가 났을 때 이를 포맷팅 하는데 영향을 줍니다. 라라벨 5.5에서는 JSON 인증 실패는 앞서 설명한 새로운 형태의 규칙을 따르는 에러 메세지를 반환합니다.

#### A Note On Form Requests
#### Form Request 주의사항

If you were customizing the response format of an individual form request, you should now override the `failedValidation` method of that form request, and throw an `HttpResponseException` instance containing your custom response:

개별 form request에서 response-응답 포맷을 커스터마이징 했었다면, 이제 개별 form request 클래스의 `failedValidation`를 오버라이드 하고, 커스텀 응답을 포함하는 `HttpResponseException` 인스턴스를 발생시켜야 합니다:

    use Illuminate\Http\Exceptions\HttpResponseException;

    /**
     * Handle a failed validation attempt.
     *
     * @param  \Illuminate\Contracts\Validation\Validator  $validator
     * @return void
     *
     * @throws \Illuminate\Validation\ValidationException
     */
    protected function failedValidation(Validator $validator)
    {
        throw new HttpResponseException(response()->json(..., 422));
    }

### Filesystem
### 파일 시스템

#### The `files` Method
#### `files` 메소드

The `files` method now returns an array of `SplFileInfo` objects, similar to the `allFiles` method. Previously, the `files` method returned an array of string path names.

`files` 메소드는 이제 `allFiles` 메소드와 비슷하게 `SplFileInfo` 객체의 배열을 반환합니다. 이전까지는 `files` 메소드는 문자형태의 경로값을 가지는 배열을 반환했었습니다.

### Mail
### 메일

#### Unused Parameters
#### 사용하지 않는 파라미터

The unused `$data` and `$callback` arguments were removed from the `Illuminate\Contracts\Mail\MailQueue` contract's `queue` and `later` methods:

`Illuminate\Contracts\Mail\MailQueue` contracts 의 `queue` 와 `later` 메소드의 `$data`와 `$callback` 인자는 제거되었습니다:

    /**
     * Queue a new e-mail message for sending.
     *
     * @param  string|array|MailableContract  $view
     * @param  string  $queue
     * @return mixed
     */
    public function queue($view, $queue = null);

    /**
     * Queue a new e-mail message for sending after (n) seconds.
     *
     * @param  \DateTimeInterface|\DateInterval|int  $delay
     * @param  string|array|MailableContract  $view
     * @param  string  $queue
     * @return mixed
     */
    public function later($delay, $view, $queue = null);

### Requests
### Requests-요청

#### The `has` Method
#### `has` 메소드

The `$request->has` method will now return `true` for empty strings and `null`. A new `$request->filled` method has been added that provides the previous behavior of the `has` method.

`$request->has` 메소드는 이제 빈 문자열 그리고 `null` 에 대해서도 `true` 값을 반환합니다. 이전의 `has` 메소드의 동작은 새로운 `$request->filled` 메소드가 대신합니다.

#### The `intersect` Method
#### `intersect` 메소드

The `intersect` method has been removed. You may replicate this behavior using `array_filter` on a call to `$request->only`:

`intersect` 메소드는 제거되었습니다. `$request->only` 호출에 `array_filter`를 사용하여 동일한 동작을 수행할 수 있습니다:

    return array_filter($request->only('foo'));

#### The `only` Method
#### `only` 메소드

The `only` method will now only return attributes that are actually present in the request payload. If you would like to preserve the old behavior of the `only` method, you may use the `all` method instead.

`only` 메소드는 이제 request payload에 실재로 존재하는 속성만 반환합니다. `only` 메소드의 이전 동작을 유지하려면 대신 `all` 메소드를 사용하면 됩니다.

    return $request->all('foo');

#### The `request()` Helper

The `request` helper will no longer retrieve nested keys. If needed, you may use the `input` method of the request to achieve this behavior:

`request` 헬퍼 함수는 더이상 중첩된 키를 조회하지 않습니다. 필요하다면, 이 동작을 위해서 request 의 `input` 메소드를 사용할 수 있습니다:

    return request()->input('filters.date');

### Testing
### 테스팅

#### Authentication Assertions
#### 인증 Assertions

Some authentication assertions were renamed for better consistency with the rest of the framework's assertions:

몇몇의 인증 관련 테스팅 확인사항들은 프레임워크의 다른 부분들과의 일관성을 위해서 이름이 변경되었습니다:

- `seeIsAuthenticated` was renamed to `assertAuthenticated`.
- `dontSeeIsAuthenticated` was renamed to `assertGuest`.
- `seeIsAuthenticatedAs` was renamed to `assertAuthenticatedAs`.
- `seeCredentials` was renamed to `assertCredentials`.
- `dontSeeCredentials` was renamed to `assertInvalidCredentials`.

- `seeIsAuthenticated` 는 `assertAuthenticated`으로 이름이 변경되었습니다.
- `dontSeeIsAuthenticated` 는  `assertGuest` 으로 이름이 변경되었습니다.
- `seeIsAuthenticatedAs` 는 `assertAuthenticatedAs` 으로 이름이 변경되었습니다.
- `seeCredentials` 는 `assertCredentials` 으로 이름이 변경되었습니다.
- `dontSeeCredentials` 는 `assertInvalidCredentials` 으로 이름이 변경되었습니다.

#### Mail Fake
#### Mail Fake

If you are using the `Mail` fake to determine if a mailable was **queued** during a request, you should now use `Mail::assertQueued` instead of `Mail::assertSent`. This distinction allows you to specifically assert that the mail was queued for background sending and not sent during the request itself.

메일이 request 를 처리하는 동안 **queue**를 통해서 처리되었는지 확인하기 위해서 `Mail` fake 를 사용하고 있었다면, `Mail::assertSent` 대신에 `Mail::assertQueued`를 사용해야 합니다. 이러한 구분방법을 통해서 메일이 백그라운드 전송을 위해서 queue에 대기중이며, request 를 처리하는 그 동안에 전송되지 않았다는 것을 확인할 수 있습니다.

### Translation
### 다국어

#### The `LoaderInterface`
#### `LoaderInterface`

The `Illuminate\Translation\LoaderInterface` interface has been moved to `Illuminate\Contracts\Translation\Loader`.

`Illuminate\Translation\LoaderInterface` 인터페이스는 `Illuminate\Contracts\Translation\Loader`로 이동되었습니다.

### Validation
### 유효성 검사

#### Validator Methods
#### Validator 메소드

All of the validator's validation methods are now `public` instead of `protected`.

validator의 모든 유효성 검사 메소드들은 이제 `protected` 대신에 `public`으로 변경되었습니다.

### Views
### Views-뷰

#### Dynamic "With" Variable Names
#### 동적 "With" 변수 이름

When allowing the dynamic `__call` method to share variables with a view, these variables will automatically use "camel" case. For example, given the following:

동적 `__call` 메소드가 뷰-view 와 변수를 공유할 때, 이 변수들은 자동으로 "카멜" 케이스 형태가 됩니다. 예를 들어 다음과 같습니다:

    return view('pool')->withMaximumVotes(100);

The `maximumVotes` variable may be accessed in the template like so:

`maximumVotes` 변수는 템플릿 안에서 다음과 같이 엑세스 할 수 있습니다:

    {{ $maximumVotes }}