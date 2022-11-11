 () => recipientChainList.find((item) => item.dp_denom === selectedRecipientChainDP)!,


이렇게 맨 뒤에 ! 써주면 해당 변수는 절대 undefine이 아니라고 강제로 해주는거라 undefine 넘어오면 터짐