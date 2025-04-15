<template>
    <div class="flex justify-center items-center h-screen text-center">
        <Form class="flex justify-center flex-col gap-4">
            <div class="flex flex-col gap-2">
                <FloatLabel variant="on">
                    <InputText id="username" />
                    <label for="username">Nome de usuário</label>
                </FloatLabel>
            </div>
            <div class="flex flex-col gap-2">
                <FloatLabel variant="on">
                    <InputText id="password" type="password"/>
                    <label for="password">Senha</label>
                </FloatLabel>
            </div>
            <Button type="submit" severity="primary" label="Entrar" />
        </Form>
    </div>
</template>