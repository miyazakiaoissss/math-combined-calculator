import streamlit as st
from sympy import sympify, expand
import re

# ページ設定
st.set_page_config(page_title="図形と式の計算", page_icon="🔢", layout="centered")

# ----------- 入力式を補正する関数 -----------
def preprocess_expression(expr):
    expr = re.sub(r'(\d)([a-zA-Z])', r'\1*\2', expr)   # 2x -> 2*x
    expr = re.sub(r'(\d)\(', r'\1*(', expr)            # 3(x+1) -> 3*(x+1)
    expr = expr.replace("^", "**")                     # x^2 -> x**2
    return expr

# ----------- 表示用：* を省略 -----------
def display_expression(expr):
    return str(expr).replace("*", "")

# ----------- 計算の実行 -----------
def calculate_combined(input_r_str, written_r_str, written_t_str):
    try:
        if not input_r_str or not written_r_str or not written_t_str:
            return "エラー", "", "", "", "空の入力があります"
        
        input_r = sympify(preprocess_expression(input_r_str))
        written_r = sympify(preprocess_expression(written_r_str))
        written_t = sympify(preprocess_expression(written_t_str))
        
        # 四角形の計算（足し算）
        result_r = written_r + input_r
        
        # 最終結果（四角形の結果 × 三角形）
        result = expand(result_r * written_t)
        
        return (display_expression(input_r), 
                display_expression(written_r), 
                display_expression(written_t), 
                display_expression(result_r),
                display_expression(result))
    except Exception as e:
        return "エラー", "", "", "", str(e)

# タイトル
st.title("🔢 図形と式の計算")
st.markdown("**計算の流れ: もとの式 → 四角形（足し算） → 三角形（掛け算） → 最終結果**")
st.markdown("---")

# 入力フィールド
col1, col2, col3 = st.columns(3)

with col1:
    input_r_str = st.text_input("もとの数や式：", placeholder="例: x+1", key="input_orig")

with col2:
    written_r_str = st.text_input("四角に書かれた数や式：", placeholder="例: 2", key="written_rect")

with col3:
    written_t_str = st.text_input("三角に書かれた数や式：", placeholder="例: x", key="written_tri")

# 計算ボタン
if st.button("🔄 計算実行", key="calc_combined"):
    input_display, written_r_display, written_t_display, result_r_display, final_result = calculate_combined(
        input_r_str, written_r_str, written_t_str
    )
    st.session_state.calc_result = (input_display, written_r_display, written_t_display, result_r_display, final_result)

# 結果表示
if hasattr(st.session_state, 'calc_result'):
    input_display, written_r_display, written_t_display, result_r_display, final_result = st.session_state.calc_result
    
    st.markdown("### 計算結果")
    
    if input_display != "エラー":
        # 正常な計算結果の表示
        st.markdown(f"""
        <div style='text-align: center; margin: 30px 0;'>
            <div style='display: flex; align-items: center; justify-content: center; gap: 20px; flex-wrap: wrap;'>
                <!-- もとの式 -->
                <div style='font-size: 18px; font-weight: bold; color: #333;'>{input_display}</div>
                
                <!-- 矢印1 -->
                <div style='font-size: 24px; color: #666;'>→</div>
                
                <!-- 四角形 -->
                <div style='display: flex; align-items: center; gap: 10px;'>
                    <div style='display: inline-block; width: 70px; height: 50px; background-color: lightblue; 
                                border: 2px solid #333; display: flex; align-items: center; justify-content: center;
                                font-size: 14px; font-weight: bold;'>
                        {written_r_display}
                    </div>
                    <div style='font-size: 16px; color: #666;'>(+)</div>
                </div>
                
                <!-- 矢印2 -->
                <div style='font-size: 24px; color: #666;'>→</div>
                
                <!-- 三角形 -->
                <div style='display: flex; align-items: center; gap: 10px;'>
                    <div style='position: relative; display: inline-block;'>
                        <div style='width: 0; height: 0; 
                                    border-left: 35px solid transparent; border-right: 35px solid transparent;
                                    border-bottom: 50px solid lightgreen; margin: 0 auto;'>
                        </div>
                        <div style='position: absolute; top: 30px; left: 50%; transform: translateX(-50%); 
                                    font-size: 14px; font-weight: bold; color: black;'>
                            {written_t_display}
                        </div>
                    </div>
                    <div style='font-size: 16px; color: #666;'>(×)</div>
                </div>
                
                <!-- 矢印3 -->
                <div style='font-size: 24px; color: #666;'>→</div>
                
                <!-- 最終結果 -->
                <div style='font-size: 20px; font-weight: bold; color: #d63384; 
                            padding: 10px 15px; background-color: #f8f9fa; border-radius: 8px; border: 2px solid #d63384;'>
                    {final_result}
                </div>
            </div>
        </div>
        """, unsafe_allow_html=True)
        
        # 計算過程の詳細表示
        st.markdown("### 📊 計算過程")
        
        col_step1, col_step2 = st.columns(2)
        
        with col_step1:
            st.markdown("**Step 1: 四角形での計算（足し算）**")
            st.markdown(f"`{input_display} + {written_r_display} = {result_r_display}`")
        
        with col_step2:
            st.markdown("**Step 2: 三角形での計算（掛け算）**")
            st.markdown(f"`({result_r_display}) × {written_t_display} = {final_result}`")
    
    else:
        # エラーの場合
        st.error(f"計算エラー: {final_result}")

# 計算例の表示
st.markdown("---")
st.markdown("### 💡 計算例")

example_col1, example_col2 = st.columns(2)

with example_col1:
    st.markdown("**例1:**")
    st.markdown("- もとの式: `x+1`")
    st.markdown("- 四角形: `2`")  
    st.markdown("- 三角形: `x`")
    st.markdown("- 結果: `(x+1+2) × x = (x+3) × x = x² + 3x`")

with example_col2:
    st.markdown("**例2:**")
    st.markdown("- もとの式: `2x`")
    st.markdown("- 四角形: `3`")
    st.markdown("- 三角形: `x+1`")
    st.markdown("- 結果: `(2x+3) × (x+1) = 2x² + 5x + 3`")

st.markdown("---")
st.markdown("### 📝 対応している記法")
st.markdown("""
- `2x` → `2*x` (自動変換)
- `x^2` → `x**2` (自動変換)  
- `3(x+1)` → `3*(x+1)` (自動変換)
- 例: `x+1`, `2x^2+3x`, `(x+2)(x-1)` など
""")
