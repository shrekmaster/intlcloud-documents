## イメージとは
Tencent Cloudイメージは、CVMインスタンスを起動するために必要な情報を提供します。必要なイメージを指定したら、対象イメージから必要な数のインスタンスを起動できます。必要に応じて複数の異なるイメージからインスタンスを起動することもできます。イメージはCVMの「インストールディスク」です。

## イメージの種類
Tencent Cloudは以下の種類のイメージ提供します。

- **パブリックイメージ：**すべてのユーザーが使用でき、主流のOSのほとんどをカバーしています。
- **サービスマーケットイメージ：**すべてのユーザーが使用でき、OSに加えて、特定のアプリケーションも統合されています。
- **カスタムイメージ：**作成者と共有オブジェクトのみが使用でき、既存の実行中のインスタンスで作成されるか、外部からインポートされます。
- **共有イメージ：**他のユーザーによって共有されるイメージで、インスタンスの作成のみに使用できます。

イメージ種類の詳細については、 [イメージ種類](/doc/product/213/4941) をご参照ください。

##イメージデプロイ VS 手動デプロイ

| |**イメージデプロイ**|**手動デプロイ** |
| ------|------ |------ |
|デプロイ時間|&nbsp;&nbsp;&nbsp;3 - 5 分|&nbsp;&nbsp;&nbsp;1 - 2日|
|デプロイプロセス|&nbsp;&nbsp;&nbsp;成熟したサービスマーケットのソリューションまたは使用実績のあるソリューションに基づいて、適切なCVMを迅速に作成します。|&nbsp;&nbsp;&nbsp;適切なOS、データベース、アプリケーションソフトウェア、プラグインなどを選択して、インストールとデバッグする必要があります。|
|セキュリティ|&nbsp;&nbsp;&nbsp;共有イメージのソースは、ユーザー自身による識別が必要があります。そのほかのパブリックイメージ、カスタムイメージ、およびサービスマーケットのイメージは、Tencent Cloudによってテスト・審査されます。|&nbsp;&nbsp;&nbsp;開発・デプロイ担当者の能力に依存します。|
|優先適用ユースケース|&nbsp;&nbsp;&nbsp;パブリックイメージ：正式OSであり、Tencent Cloudが提供する初期化コンポーネントが含まれています。<br>&nbsp;&nbsp;&nbsp;サービスマーケットイメージ：成熟した構築ソリューションに基づいて、パーソナライズされたアプリケーション環境を迅速に構築します。<br>&nbsp;&nbsp;&nbsp;カスタムイメージ：既存のCVMと同じソフトウェア環境を迅速に作成し、または、環境のバックアップを作成します。<br>&nbsp;&nbsp;&nbsp;共有イメージ：他のユーザーの既存のCVMと同じソフトウェア環境を迅速に作成します。| &nbsp;&nbsp;&nbsp;独自構成であり、基本設定はありません。|

## イメージアプリケーション
 - **特定のソフトウェア環境のデプロイ**
共有イメージ、カスタムイメージ、およびサービスマーケットイメージを使用して、特定のソフトウェア環境を迅速に構築できます。独自で環境を構成する必要もなく、ソフトウェアのインストールなどの面倒で時間のかかる作業も不要です。またサイト構築、アプリケーション開発、可視化管理などのパーソナライズされた要件を満たすことができます。CVMを「すぐに利用可能」にし、時間の節約と利便性を図れます。

 - **ソフトウェア環境の一括デプロイ**
ソフトウェア環境がデプロイ済みのCVMインスタンスをイメージに作成してから、CVMインスタンスを一括作成するときに作成したイメージをOSとして使用することで、CVMインスタンスが正常に作成された後、以前のCVMインスタンスと同じソフトウェア環境になりますので、ソフトウェア環境を一括デプロイすることは可能になります。

 - **サーバー動作環境のバックアップ**
CVMインスタンスが使用中にソフトウェア障害が発生し、正常に動作できなくなった場合は、イメージを使用してCVMインスタンスを回復することができます。

##イメージのライフサイクル

次の図は、カスタムイメージのライフサイクルをまとめたものです。新しいカスタムイメージが作成またはインポートされると、ユーザーはそれを使用して新しいインスタンスを起動できます（ユーザーは既存のパブリックイメージまたはサービスマーケットイメージからインスタンスを起動することもできます）。カスタムイメージは、同じアカウントの他のリージョンにコピーして、当該リージョンの独立したイメージになります。ユーザーは、カスタムイメージを他のユーザーと共有することもできます。
![](//mc.qcloudimg.com/static/img/796e37b9837f2b102c4beaa45e9c13ca/image.png)


