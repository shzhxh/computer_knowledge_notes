- 指令类型
  - R型：register型，从寄存器中取出源操作数，运算结果再写入寄存器中  
    ADDU,ADD,CMP,MFIH,MFPC,MOVE
  - I型：immediate型 ，使用立即数作为源操作数  
    ADDIU,ADDIU3,ADDSP3,ADDSP,CMPI,INT,LI,LW,LW_SP
  - B型：branch型，转移型  
    B,BEQZ,BNEZ,BTEQZ,BTNEZ
  - J型：jump型，长跳转型  
    JALR,JR,JRRA
- 功能分类
  - 算术/逻辑运算
    1. `ADDIU rx immediate` (add immediate unsigned)
    2. `ADDIU3 rx ry immediate`
    3. `ADDSP3 rx immediate`
    4. `ADDSP immediate`
    5. `ADDU rx ry rz`
    40. `SUBU rx ry rz`
    26. `NEG rx ry` (negative)
    12. `CMP rx ry`
    13. `CMPI rx immediate`
    6. `AND rx ry`
    29. `OR rx ry`
    27. `NOT rx ry`
    44. `XOR rx ry`
    30. `SLL rx ry immediate`
    31. `SLLV rx ry`
    32. `SLT rx ry`
    33. `SLTI rx immediate`
    34. `SLTU rx ry`
    35. `SLTUI rx immediate`
    36. `SRA rx ry immediate`
    37. `SRAV rx ry`
    38. `SRL rx ry immediate`
    39. `SRLV rx ry`
  - 分支操作
    7. `B immediate`
    8. `BEQZ rx immediate`
    9. `BNEZ rx immediate`
    10. `BTEQZ immediate`
    11. `BTNEZ immediate`
    14. `INT immediate`
    15. `JALR rx` (jump and link)
    16. `JR rx` (jump register)
    17. `JRRA` (jump register RA)
  - 加载和存储
    18. `LI rx immediate` (load immediate)
    19. `LW rx ry immediate`(load indirect addressing)
    20. `LW_SP rx immediate`
    41. `SW rx ry immediate`
    42. `SW_RS immediate`
    43. `SW_SP rx immediate`
  - 数据传送
    21. `MFIH rx` (move from IH)
    22. `MFPC rx` (move from PC)
    23. `MOVE rx ry`
    24. `MTIH rx`
    25. `MTSP rx`
  - 空操作
    28. `NOP` (空操作)
