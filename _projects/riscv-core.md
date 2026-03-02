---
layout: page
title: RISC-V RV32I Core Design
description: SystemVerilog를 활용한 5-Stage 파이프라인 프로세서 설계 및 검증
img: assets/img/12.jpg # 임시 이미지 (나중에 수혁님 사진으로 교체)
importance: 1
category: Hardware Design
---

## 1. 프로젝트 개요
SystemVerilog를 사용하여 기본 명령어 셋(RV32I)을 지원하는 32비트 프로세서 코어를 직접 설계하고 시뮬레이션 환경에서 검증한 프로젝트입니다.

- **기간:** 202X.XX ~ 202X.XX
- **언어 및 툴:** SystemVerilog, Vivado, Synopsys VCS
- **핵심 목표:** 파이프라인 구조 이해 및 데이터 해저드(Data Hazard) 해결

<br>

## 2. 아키텍처 및 모듈 설계
ALU, 레지스터 파일, 컨트롤 유닛 등을 모듈화하여 설계했습니다. 아래는 데이터 해저드를 해결하기 위해 작성한 포워딩 유닛(Forwarding Unit)의 코드 일부입니다.

```systemverilog
module forwarding_unit (
    input logic [4:0] rs1, rs2,
    input logic [4:0] ex_mem_rd, mem_wb_rd,
    input logic ex_mem_regwrite, mem_wb_regwrite,
    output logic [1:0] forward_a, forward_b
);
    // Forward A logic
    always_comb begin
        if (ex_mem_regwrite && (ex_mem_rd != 0) && (ex_mem_rd == rs1))
            forward_a = 2'b10;
        else if (mem_wb_regwrite && (mem_wb_rd != 0) && (mem_wb_rd == rs1))
            forward_a = 2'b01;
        else
            forward_a = 2'b00;
    end
endmodule


