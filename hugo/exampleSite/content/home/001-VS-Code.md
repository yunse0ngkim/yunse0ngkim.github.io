+++
title = "Opensource Contribution with VS Code embeded Git Utility"
outputs = ["Reveal"]
[logo]
src = "github-logo.png"
[reveal_hugo]
custom_theme = "reveal-hugo/themes/robot-lung.css"
margin = 0.2
weight = 10
+++

## VS Code 를 활용한 Git

- 마이크로소프트 윈도우, macOS, 리눅스용으로 개발한 소스 코드 편집기이다.
- Plugin 을 추가하여 풍부한 기능을 가진 IDE 로 사용할 수 있다.
- VS Code 자체적으로 지원하는 Git 기능
  - 철권 중급자 수준의 복잡한 커맨드를 기능 버튼으로 함축했습니다.
- 추가적으로 Git 기능 외에도 SSH 또는 Live Share 원격 개발 환경 및 멀티 유저 편집이 가능합니다.

<img src="0010-git-cheetsheet.png" alt="그림 2 : Git 커맨드 요약본" width=400>
<img src="0010-vscode-git.png" alt="그림 1 : VS Code 에서 Git" width=400>

---

## Visual Studio Code 플러그인 Git History

- History 그래프를 볼 때 유용한 Git History 플러그인
- 커밋 히스토리를 보기 위한 `git log`, 특정 커밋의 시점으로 바꾸는 `git checkout` 에 대응하는 기능인데 직관적으로 보기 편합니다.

<br>

<iframe name="git-history" src="https://youtu.be/hW6mhIaZjgw" data-autoplay></iframe>

---

## Visual Studio Code 플러그인 Git History

- 코드 라인 & 파일 별 히스토리 추적에 용이한 Git Lens 플러그인

<br>

<iframe name="git-lens" src="https://youtu.be/WzIpMlS89HI" data-autoplay></iframe>

---

## 빠르게 훑어 보는 오픈소스 컨트리뷰션(Uftrace) 루틴 1 - Issue Check Commit

코드 정적 분석기로 나온 항목 중에 Null 관련 보안 사항이 있었습니다.
```patch
cmds/graph.c:282: error: Null Dereference
  pointer `graph` last assigned on line 279 could be null and is dereferenced at line 282, column 3.
  280.
  281.  if (tg->utg.graph && tg->utg.graph != graph) {
  282.          pr_dbg("detect new session: %.*s\n", SESSION_ID_LEN, graph->sess->sid);
         ^
  283.          tg->utg.new_sess = true;
  284.  }
```

깃허브 이슈로 등록한 뒤에 해당 사항을 보완하는 작업을 진행합니다.

<iframe name="commit" src="https://youtu.be/JDUcRbE1AsU" data-autoplay></iframe>

---

## 빠르게 훑어 보는 Github 오픈소스(Uftrace) 컨트리뷰션 루틴 2 - Before PR

커밋을 하고 난 뒤에 `마지막 커밋 실행 취소`로 쉽게 스테이징 단계로 되돌아 올 수 있습니다.

<iframe name="back-commit" src="https://youtu.be/TIVsQCtw9ME" data-autoplay></iframe>

---

## 빠르게 훑어 보는 Github 오픈소스(Uftrace) 컨트리뷰션 루틴 2 - Before PR

커밋하고 푸쉬로 origin 브랜치에 반영되었는데 또 수정이 필요하다면?  
커밋을 다시할 필요 없이 마지막 커밋을 `git push -f` 로 덮어 써줍니다.  

<img src="0022-commit-rollback-update-origin-branch.png" alt="Fcommit-rollback-update-origin-branch.png">

---

## 빠르게 훑어 보는 Github 오픈소스(Uftrace) 컨트리뷰션 루틴 3 - PR

fork 한 브랜치(Downstream)가 Upstream 에 머지할 수 있도록 PR 을 작성하는 단계입니다.

<iframe name="PR" src="https://youtu.be/wr6dkL0FGPA" data-autoplay></iframe>

---

## 빠르게 훑어 보는 메일링 리스트 기반 오픈소스(Linux Kernel) 컨트리뷰션 루틴 A - Patch

github 와 1 ~ 2 와 유사하지만, PR 과 코드 리뷰를 모두 메일로 보냅니다.

```bash
git send-email --smtp-pass="비밀번호" --to="메인테이너@이메일.주소" --cc="참조할@메일.주소들" --confirm=always -M -1
```
뒤에 카운트는 작업한 커밋 개수를 넣습니다. 

예시
```bash
git send-email --to="Thomas Gleixner <tglx@linutronix.de>, Marc Zyngier <maz@kernel.org>" --cc="linux-kernel@vger.kernel.org, Austin Kim <austindh.kim@gmail.com>" --confirm=always -M -1
```

커밋 메세지를 작성할 때, 영어 작문 실력이 필요하지만, 우리에겐 구글 번역기가 있습니다.

```patch
Hello

Since we have a macro defined in our IRQ subsystem internal functions to
traverse the list of actions, how about refactoring this loop?

- genirq: Use a common macro to go through the actions list
(f944b5a7aff05a244a6c8cac297819af09a199e4)

have a good day!

---
 kernel/irq/irqdesc.c | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/kernel/irq/irqdesc.c b/kernel/irq/irqdesc.c
index 939d21cd55c3..34a0cefff712 100644
--- a/kernel/irq/irqdesc.c
+++ b/kernel/irq/irqdesc.c
@@ -246,12 +246,12 @@ static ssize_t actions_show(struct kobject *kobj,
 			    struct kobj_attribute *attr, char *buf)
 {
 	struct irq_desc *desc = container_of(kobj, struct irq_desc, kobj);
-	struct irqaction *action;
+	struct irqaction *action = NULL;
 	ssize_t ret = 0;
 	char *p = "";
 
 	raw_spin_lock_irq(&desc->lock);
-	for (action = desc->action; action != NULL; action = action->next) {
+	for_each_action_of_desc(desc, action) {
 		ret += scnprintf(buf + ret, PAGE_SIZE - ret, "%s%s",
 				 p, action->name);
 		p = ",";
-- 
```

---

## 빠르게 훑어 보는 메일링 리스트 기반 오픈소스(Linux Kernel) 컨트리뷰션 루틴 B - Code Review

보낸 패치에 대해서 보완해야할 사항을 해당 서브시스템의 메인테이너가 말해줍니다.

```patch
Re: [PATCH] genirq: Refactor actions_show loop block using a common macro to go through the actions list
    - by Thomas Gleixner @ 2022-04-10 19:17 UTC [7%]

On Fri, Apr 08 2022 at 20:41, you wrote:

thanks for providing this patch.

> Hello.
>
> Since we have a macro defined in our IRQ subsystem internal functions to
> traverse the list of actions, how about refactoring this loop?
>
> - genirq: Use a common macro to go through the actions list
> (f944b5a7aff05a244a6c8cac297819af09a199e4)
>
> have a good day!

Neither 'Hello' nor 'have a good day' are part of the change log.

Also please write the changelog in a factual way and not in form of a
question. If you want to add a reference to a git commit, then please
use the canonical form as described in Documentation/process, where you
also find the general patch submission rules. There is also a tip tree
specific chapter:

https://www.kernel.org/doc/html/latest/process/maintainer-tip.html?highlight=x86#patch-submission-notes

Following these rules makes everyones life simpler.

> ---
>  kernel/irq/irqdesc.c | 4 ++--
>  1 file changed, 2 insertions(+), 2 deletions(-)
>
> diff --git a/kernel/irq/irqdesc.c b/kernel/irq/irqdesc.c
> index 939d21cd55c3..34a0cefff712 100644
> --- a/kernel/irq/irqdesc.c
> +++ b/kernel/irq/irqdesc.c
> @@ -246,12 +246,12 @@ static ssize_t actions_show(struct kobject *kobj,
>  			    struct kobj_attribute *attr, char *buf)
>  {
>  	struct irq_desc *desc = container_of(kobj, struct irq_desc, kobj);
> -	struct irqaction *action;
> +	struct irqaction *action = NULL;

There is no NULL initialization required.

Thanks,

        tglx
```

---

## 빠르게 훑어 보는 메일링 리스트 기반 오픈소스(Linux Kernel) 컨트리뷰션 루틴 B - Resend Version

보완한 패치를 다시 보냅니다. 서브시스템의 메인테이너가 말해줍니다.

```patch
Refactor for loop to macro for_each_action_of_desc

Signed-off-by: Paran Lee <p4ranlee@gmail.com>
---
 kernel/irq/irqdesc.c | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/kernel/irq/irqdesc.c b/kernel/irq/irqdesc.c
index d323b180b0f3..5db0230aa6b5 100644
--- a/kernel/irq/irqdesc.c
+++ b/kernel/irq/irqdesc.c
@@ -251,7 +251,7 @@ static ssize_t actions_show(struct kobject *kobj,
 	char *p = "";
 
 	raw_spin_lock_irq(&desc->lock);
-	for (action = desc->action; action != NULL; action = action->next) {
+	for_each_action_of_desc(desc, action) {
 		ret += scnprintf(buf + ret, PAGE_SIZE - ret, "%s%s",
 				 p, action->name);
 		p = ",";
```

---

## 빠르게 훑어 보는 메일링 리스트 기반 오픈소스(Linux Kernel) 컨트리뷰션 루틴 B - Merged

보낸 패치가 v6.0-rc1 에서 머지되었습니다.

[GIT pull] irq/core for v6.0-rc1
    - by Thomas Gleixner @ 2022-08-01 14:48 UTC [1%]

```patch
The following commit has been merged into the irq/irqchip-next branch of irqchip:

Commit-ID:     c904cda04482d5ab545e5a82cee6084078ef9543
Gitweb:        https://git.kernel.org/pub/scm/linux/kernel/git/maz/arm-platforms/c904cda04482d5ab545e5a82cee6084078ef9543
AuthorDate:    Sun, 10 Jul 2022 20:26:14 +09:00
Committer:     Marc Zyngier <maz@kernel.org>
CommitterDate: Wed, 20 Jul 2022 15:21:32 +01:00

genirq: Use for_each_action_of_desc in actions_show()

Refactor action_show() to use for_each_action_of_desc instead
of a similar open-coded loop.

[maz: reword commit message]
Signed-off-by: Marc Zyngier <maz@kernel.org>
Link: https://lore.kernel.org/r/20220710112614.19410-1-p4ranlee@gmail.com
---
 kernel/irq/irqdesc.c | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/kernel/irq/irqdesc.c b/kernel/irq/irqdesc.c
index d323b18..5db0230 100644
--- a/kernel/irq/irqdesc.c
+++ b/kernel/irq/irqdesc.c
@@ -251,7 +251,7 @@ static ssize_t actions_show(struct kobject *kobj,
 	char *p = "";
 
 	raw_spin_lock_irq(&desc->lock);
-	for (action = desc->action; action != NULL; action = action->next) {
+	for_each_action_of_desc(desc, action) {
 		ret += scnprintf(buf + ret, PAGE_SIZE - ret, "%s%s",
 				 p, action->name);
 		p = ",";
```

---

# 🤗

고생하셨습니다!
