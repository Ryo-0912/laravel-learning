## oldヘルパーの仕組み
[参照URL](https://www.larajapan.com/2020/09/25/old%E3%83%98%E3%83%AB%E3%83%91%E3%83%BC/)

内容を簡単に言うと、セッション```_old_input```というキーに値が入って、そこの値を読み取ることでoldヘルパーが機能する。

oldヘルパーが機能しない例として、javascriptなどでページ遷移することなどがある。これをoldヘルパーを使って、値を保持するには、例えば以下のようにする。

## register-public.blade.php
```
@extends('web.layouts.auth-base')

@section('content')
@include('web.layouts.registration-header', ['headerString' => '基本情報登録'])
<main id="main" role="main" class="create_account createPage">
  <div class="smlWhiteBg">
			<form method="POST" action="{{ route('web.store.app-profile')}}" class="createForm createForm02" enctype="multipart/form-data">
        @csrf
				<div class="comBtn pc"><a href="#" onclick="history.back(); return false;" class="backLink">{{__('labels.registration.back_step')}}</a></div>
				<p class="topTxt01 text-center">{{__('labels.registration.fill_in')}}<br><small>{{__('labels.registration.will_be_published')}}</small></p>
				<div class="img text-center"><img src="{{asset('assets/img/web/create_account/account_img01.jpg')}}" width="79" alt=""></div>
				<input class="tmp-img" type="hidden" name="tmp_img" value="">
				<div class="selectFile"><label><input type="file" id="publicImage" name="image" accept=".jpg, .jpeg, .png"><span>写真を追加</span></label></div>
                    @foreach ($errors->get('image') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
				<table>
					<tr>
						<th>{{__('labels.registration.nick_name')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td><input class="form-control" type="text" id="publicNickname" name="nickname" value="{{ old('nickname') }}" placeholder="太宰 太郎" maxlength="20" required></td>
					</tr>
					@foreach ($errors->get('nickname') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th>自己紹介</th>
					</tr>
					<tr>
						<td><textarea class="form-control" id="publicBiography" name="biography" placeholder="得意科目は英語です。よろしくお願いします。" maxlength="1000">**{{old('biography')}}**</textarea></td>
					</tr>
					@foreach ($errors->get('biography') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th>タグ(最大10個設定できます)</th>
					</tr>
					<tr>
						<td>
							<ul class="tagList tagList02">
								@for ($i = 0; $i < 10; $i++)
									<li>#<input class="form-control form-control-tag" type="text" id='publicTags[{{$i}}]' name="tag_names[]" value="{{ old("tag_names.{$i}") }}" placeholder="タグ名を入力して下さい" maxlength="40"></li>
								@endfor
							</ul>
							@error('tag_names.*')
								<label class="form-text text-danger error">{{ $message }}</label>
							@enderror
						</td>
					</tr>
				</table>
				<ul class="submit">
					<li>
						<input type="submit" class="btn btn-primary" value="入力確認">
					</li>
				</ul>
			</form>
		</div>
	</main>
@endsection

@section('javascript')
    <script>
    $('.createForm .selectFile input').on('change', function(e) {
        var file = e.target.files[0];

        var fileReader = new FileReader();
        fileReader.onload = function() {

            var dataUri = this.result;
            $('.tmp-img').val(dataUri);
            $('.createForm .img img').attr('src', dataUri);

            sessionStorage.setItem('publicImage', dataUri);
        }
        fileReader.readAsDataURL(file);
    });

    $('input[name="nickname"]').on('change', function() {
        sessionStorage.setItem('publicNickname',this.value);
    });

    $('textarea[name="biography"]').on('change', function() {
        sessionStorage.setItem('publicBiography',this.value);
    });

    $('input[name="tag_names[]"]').on('change', function() {
        var id = $(this).attr('id');
        sessionStorage.setItem(`${id}`,this.value);
    });

    Object.keys(sessionStorage).forEach(function(key) {
        if (key === 'publicImage' && sessionStorage.getItem(key)) {
            $('.createForm .img img').attr('src',sessionStorage.getItem(key));
        }
        if (key != 'publicImage'){
            document.getElementById(key).value = sessionStorage.getItem(key);
        }
    });
	</script>
@endsection
```

## register-private.blade.php
```
@extends('web.layouts.auth-base')

@section('content')
@include('web.layouts.registration-header', ['headerString' => '基本情報登録', 'backLink' => 'logout'])
	<main id="main" role="main" class="create_account createPage">
		<div class="smlWhiteBg">
			<form id="logout" action="{{ route('web.logout') }}" method="POST" style="display: none;">
				@csrf
			</form>
			<form method="POST" action="{{ route('web.store.main-profile')}}" class="createForm">
				@csrf
				<div class="comBtn pc"><a href="#" onclick="event.preventDefault(); document.getElementById('logout').submit();" class="backLink">{{__('labels.registration.logout')}}</a></div>
				<p class="topTxt01 text-center">{{__('labels.registration.fill_in')}}<br><small>{{__('labels.registration.not_be_published')}}</small></p>
				<table>
                    <tr>
                        <th class="row-2">{{__('labels.teachers.last_name_kanji')}}<span class="must">{{__('labels.registration.required')}}</span></th>
                        <td class="row-2 sp">
                            <input class="form-control" id="sp-last-kanji" type="text" name="last_name_kanji" value="{{old('last_name_kanji')}}" placeholder="太宰" maxlength="20" required>
                        </td>
                        <th class="row-2">{{__('labels.teachers.first_name_kanji')}}<span class="must">{{__('labels.registration.required')}}</span></th>
                    </tr>
                    <tr>
                        <td class="row-2 pc"><input class="form-control" id="pc-last-kanji" type="text" name="last_name_kanji" value="{{old('last_name_kanji')}}" placeholder="太宰" maxlength="20" required></td>
                        <td class="row-2"><input class="form-control" type="text" name="first_name_kanji" value="{{old('first_name_kanji')}}" placeholder="太郎" maxlength="20" required></td>
                    </tr>
					@foreach ($errors->get('last_name_kanji') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					@foreach ($errors->get('first_name_kanji') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
                    <tr>
                        <th class="row-2">{{__('labels.teachers.last_name_kana')}}<span class="must">{{__('labels.registration.required')}}</span></th>
                        <td class="row-2 sp"><input class="form-control" id="sp-last-kana" type="text" name="last_name_kana" value="{{old('last_name_kana')}}" placeholder="ダザイ" maxlength="20" required></td>
                        <th class="row-2">{{__('labels.teachers.first_name_kana')}}<span class="must">{{__('labels.registration.required')}}</span></th>
                    </tr>
                    <tr>
                        <td class="row-2 pc"><input class="form-control" id="pc-last-kana" type="text" name="last_name_kana" value="{{old('last_name_kana')}}" placeholder="ダザイ" maxlength="20" required></td>
                        <td class="row-2"><input class="form-control" type="text" name="first_name_kana" value="{{old('first_name_kana')}}" placeholder="タロウ" maxlength="20" required></td>
                    </tr>
					@foreach ($errors->get('last_name_kana') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					@foreach ($errors->get('first_name_kana') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.account')}}<br>{{__('labels.registration.zip_code')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="zip_code" value="{{old('zip_code')}}" onkeyup="AjaxZip3.zip2addr('zip_code','','prefecture_cd','city');" onkeypress="return onlyNumberKey(event)" maxlength="7" required><div class="form-btn"><button>{{__('labels.registration.display_account')}}</button></div><br>{{__('labels.registration.enter_without_hyphens')}}<br><span class="img"><img src="{{asset('assets/img/web/common/select_bg02.png')}}" width="8" alt=""></span>{{__('labels.registration.automatically_display')}}</td>
					</tr>
					@foreach ($errors->get('zip_code') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.prefecture')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td colspan="2">
							<select class="form-select" name="prefecture_cd">
								@foreach(__('prefecture_cd') as $pref_id => $name)
									<option value="{{$pref_id}}" @selected(old('prefecture_cd') == $pref_id)>{{$name}}</option>
								@endforeach
							</select>
						</td>
					</tr>
					@foreach ($errors->get('prefecture_cd') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.city')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="city" value="{{old('city')}}" placeholder="渋谷区代々木" maxlength="20" required></td>
					</tr>
					@foreach ($errors->get('city') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.street_number')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="street_number" value="{{old('street_number')}}" placeholder="1丁目2-3" maxlength="40" required></td>
					</tr>
					@foreach ($errors->get('street_number') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.building_name')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="building_name" value="{{old('building_name')}}" placeholder="マンションの場合は部屋番号までを入力して下さい" maxlength="40"></td>
					</tr>
					<tr>
						<th colspan="2">{{__('labels.registration.phone_number')}}<span class="must">{{__('labels.registration.required')}}</span></th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="tel" name="phone" name="input class="form-control"_main_tp" value="{{old('phone')}}" placeholder="01234567890" maxlength="11" onkeypress="return onlyNumberKey(event)" required>{{__('labels.registration.enter_without_hyphens')}}</td>
					</tr>
					@foreach ($errors->get('phone') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.employee_number')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="employee_no" value="{{old('employee_no')}}" maxlength="6" onkeypress="return onlyNumberKey(event)"><p class="notes" style="color:red;">{{__('labels.registration.employee_number_explanation')}}</p><p class="notes">{{__('labels.registration.employee_number_6digits')}}</p></td>
					</tr>
					@foreach ($errors->get('employee_no') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.payee_information')}}<br>{{__('labels.registration.bank_name')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="name" name="bank_name" value="{{old('bank_name')}}" placeholder="金融機関名を入力して下さい" maxlength="20">
						<p class="notes">{{__('labels.registration.contact_company_1')}}<a href="{{route('web.inquiry.index')}}" style="color:blue">{{__('labels.registration.inquiry')}}</a>{{__('labels.registration.contact_company_2')}}</p>
						</td>
					</tr>
					@foreach ($errors->get('bank_name') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.branch_name')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="name" name="branch_name" value="{{old('branch_name')}}" placeholder="支店名を入力して下さい" maxlength="30"></td>
					</tr>
					@foreach ($errors->get('branch_name') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.account_type')}}</th>
					</tr>
					<tr>
						<td colspan="2">
							<select class="form-select" name="account_type">
								<option value="">-</option>
								@foreach (App\Enums\BankAccountType::cases() as $bankType)
									<option value="{{ $bankType->value }}" @selected(old('account_type') == $bankType->value)>{{ $bankType->label() }}</option>
								@endforeach
							</select>
						</td>
					</tr>
					@foreach ($errors->get('account_type') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.account_name')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="account_name" value="{{old('account_name')}}" placeholder="タナカ タロウ" maxlength="40">
						<p class="notes">{{__('labels.registration.double_byte_char')}}<br>{{__('labels.registration.put_a_space')}}</p>
						</td>
					</tr>
					@foreach ($errors->get('account_name') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
					<tr>
						<th colspan="2">{{__('labels.registration.account_number')}}</th>
					</tr>
					<tr>
						<td colspan="2"><input class="form-control" type="text" name="account_number" value="{{old('account_number')}}" placeholder="1234567" maxlength="7" onkeypress="return onlyNumberKey(event)"><p class="notes">{{__('labels.registration.account_number_7digits')}}<br>{{__('labels.registration.fill_in_the_head_with_zeros')}}<br>{{__('labels.registration.account_number_example')}}</p></td>
					</tr>
					@foreach ($errors->get('account_number') as $error)
						<td class="row-2"><label class="text-danger">{{ $error }}</label></td>
					@endforeach
				</table>
				<ul class="submit">
					<li>
						<input type="submit" class="btn btn-primary" value="次へ">
					</li>
				</ul>
			</form>
		</div>
	</main>
@endsection

<style lang="scss">
  .createForm .form-control[name="zip_code"] {
	width: 80px !important;
	display: inline-block !important;
}
</style>
@section('javascript')
  <script src="https://ajaxzip3.github.io/ajaxzip3.js" charset="UTF-8"></script>
  <script>
    $(function(){
      $('.createForm .form-btn button').on('click',function(){
        AjaxZip3.zip2addr(this,'zip_code','area','address01');
        return false;
      })
    })
    const mediaQuery = window.matchMedia('(min-width: 897px)');
    handle(mediaQuery);
    mediaQuery.addListener(handle);
    function handle(mm) {
    if (mm.matches) {
        document.getElementById('sp-last-kanji').disabled = true;
        document.getElementById('sp-last-kana').disabled = true;
    } else {
        document.getElementById('pc-last-kanji').disabled = true;
        document.getElementById('pc-last-kana').disabled = true;
    }
    }
  </script>
@endsection
```

## controller
```
public function showProfileForm()
{
    if(session()->get('main'))
    {
       session()->put(_old_input)
    }
    return view('web.teacher.register.register-private');
}

public function storeProfile(ProfileFormRequest $request)
{
   $request->session()->put('main', $request->only([
        'last_name_kanji', 'first_name_kanji', 'last_name_kana', 'first_name_kana',
        'zip_code', 'prefecture_cd', 'city', 'street_number', 'building_name',
        'phone', 'bank_name', 'branch_name', 'account_type', 'account_name', 'account_number',
    ]));

    return redirect(route('web.app-profile.form'));
}

public function showAppProfileForm(Request $request)
{
    if ($request->session()->missing('main')) {
        abort(500);
    }

    return view('web.teacher.register.register-public');
}
```

