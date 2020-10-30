# 제 3차 Aiffel 해커톤
Lpoint ML Project in Aiffel 🛒

## 공모전 데이터
- 총 4개의 데이터를 받았습니다.
  - 온라인 행동 정보(Behavior Table) : 업종별로 2019.07~2019.09까지 고객들의 온라인 행동 정보

    - CLNT_ID(클라이언트ID), SESS_ID(세션 ID), HIT_SEQ(조회일련번호), ACTION_TYPE(행동유형), BIZ_UNIT(업종단위), SESS_DT(세션일자), HIT_TM(조회시각), HIT_PSS_TM(조회경과시간), TRANS_ID(거래 ID), SECH_KWD(검색 키워드), TOT_PAG_VIEW_CT(총페이지조회건수), TOT_SESS_HR_V(총세션시간값), TRFC_SRC(유입채널), DVC_CTG_NM(기기유형)
 - 거래 정보(Transaction Table) : 온,오프라인 업종별로 2019.07~2019.09까지 고객들의 구매 정보

    - CLNT_ID(클라이언트ID), TRANS_ID(거래 ID), TRANS_SEQ(거래일련번호), BIZ_UNIT(업종단위), PD_C(상품소분류코드), DE_DT(구매일자), DE_TM(구매시각), BUY_AM(구매금액), BUY_CT(구매수량)
  - 고객 Demographic 정보 : 고객들의 id와 성별, 연령대 정보

    - CLNT_ID(클라이언트ID), CLNT_GENDER(성별), CLNT_AGE(연령대)
  - 상품 분류 정보 : 상품 정보(대분류,중분류,소분류 등)

    - PD_C(상품 소분류코드),CLAC1_NM(상품 대분류명),CLAC2_NM(상품 종분류명), CLAC3_NM(상품 소분류명)
